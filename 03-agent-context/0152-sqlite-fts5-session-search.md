---
id: "dsh-note-0152"
title: "SQLite FTS5 session search"
status: "implemented"
class: "feature"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/feature/2026-07-10-sqlite-session-query-provider.md"
implementation_evidence: "medium"
target_anchor: "Rust provider-adapter boundary"
tags:
  - "class/feature"
  - "concern/boundary"
  - "concern/cancellation-timeout"
  - "concern/compatibility"
  - "concern/concurrency"
  - "concern/discovery-routing"
  - "concern/evidence"
  - "concern/lifecycle"
  - "concern/ownership"
  - "concern/recovery"
  - "concern/schema-types"
  - "domain/llm"
  - "domain/observability"
  - "domain/session-state"
  - "domain/shell-terminal"
  - "domain/storage"
  - "domain/testing"
  - "lifecycle/implemented"
  - "mechanism/generation"
  - "mechanism/projection"
  - "mechanism/registry"
  - "mechanism/streaming"
aliases:
  - "sessions"
  - "searchSessions"
  - "foldSurface"
  - "createdAt"
  - "sessionQuery"
  - "sessionFilters"
  - "query"
  - "limit"
  - "SessionSearchCursor"
  - "current"
  - "SessionResultFilter"
  - "SessionEventResultFilter"
  - "SessionEventSearchHit"
  - "SessionSearchHit"
search_regex: "(?i)(sessions|searchSessions|foldSurface|createdAt|sessionQuery|sessionFilters|query|limit)"
---

# 0152. SQLite FTS5 session search — implementation context

## Open this when

The exact-read ctx.sessionQuery service deliberately has no derived index. Large persisted histories need full-text search without scanning every event on every query, while current live sessions need an overlay newer than the last durability checkpoint. Search also needs concrete ranking, snippets, filters, pagination, cancellation, and rebuild behavior. Splitting those concerns across a provider coordinator and a database implementation would create two coupled reconciliation state machines.

## Source decision

@deepseek-ai/dsh-session-query declares one abstract ctx.sessionQuery service whose exact reads, filters, and traces are concrete and whose two full-text methods are abstract. searchSessions(request, exec?) returns cursor-paginated SessionSearchHits grouped by each session's strongest matching event; searchEvents(request, exec?) returns SessionEventSearchHits within one logical session. Both requests require query, accept limit and an owned branded SessionSearchCursor, and support an optional abort signal.

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/feature/2026-07-10-sqlite-session-query-provider.md](../02-notes/implemented/feature/2026-07-10-sqlite-session-query-provider.md)
- Pinned source: [.agents/notes/implemented/feature/2026-07-10-sqlite-session-query-provider.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/feature/2026-07-10-sqlite-session-query-provider.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`packages/session-query/session-query/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/session-query/session-query/src/index.ts) | package entry point | Core file in the package named by the note: `packages/session-query/session-query`. | `named-package-member` |
| [`packages/session-query/session-query/src/types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/session-query/session-query/src/types.ts) | public types and contract | Core file in the package named by the note: `packages/session-query/session-query`. Defines `SessionSearchHit`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/session-query/session-query/src/cursor.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/session-query/session-query/src/cursor.ts) | runtime implementation | Core file in the package named by the note: `packages/session-query/session-query`. Defines `SessionSearchCursor`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/session-query/session-query/src/tracing.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/session-query/session-query/src/tracing.ts) | runtime implementation | Core file in the package named by the note: `packages/session-query/session-query`. Defines `current`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/session-query/session-query/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/session-query/session-query/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/session-query/session-query`. | `named-package-member` |
| [`packages/session-query/session-query-sqlite/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/session-query/session-query-sqlite/src/index.ts) | package entry point | Core file in the package named by the note: `packages/session-query/session-query-sqlite`. | `named-package-member` |
| [`packages/session-query/session-query-sqlite/src/query.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/session-query/session-query-sqlite/src/query.ts) | runtime implementation | Core file in the package named by the note: `packages/session-query/session-query-sqlite`. Defines `query`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/session-query/session-query-sqlite/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/session-query/session-query-sqlite/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/session-query/session-query-sqlite`. | `named-package-member` |
| [`packages/session-query/session-query`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/packages/session-query/session-query) | package or module directory | The note names this package or capability. | `named-package` |
| [`packages/session-query/session-query-sqlite`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/packages/session-query/session-query-sqlite) | package or module directory | The note names this package or capability. | `named-package` |
| [`packages/acp/acp/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/acp/acp/src/index.ts) | package entry point | Defines `sessions`, a construct named by the note. | `symbol-definition` |
| [`packages/goal/goal/src/fold.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/goal/goal/src/fold.ts) | runtime implementation | Defines `createdAt`, a construct named by the note. | `symbol-definition` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `sessions` | `const` | [`packages/acp/acp/src/index.ts:110`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/acp/acp/src/index.ts#L110) | `const sessions = new Map<SessionId, SessionRecord>()` |
| `searchSessions` | `const` | [`packages/client/ui-workspace/src/client/index.ts:56`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-workspace/src/client/index.ts#L56) | `const searchSessions: WorkspaceBrowserInjected['searchSessions'] = async (query, signal) => {` |
| `foldSurface` | `function` | [`packages/core/session/src/surface.ts:387`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/surface.ts#L387) | `export function foldSurface(events: readonly SessionEvent[]): SurfaceFoldResult {` |
| `createdAt` | `const` | [`packages/goal/goal/src/fold.ts:160`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/goal/goal/src/fold.ts#L160) | `const createdAt = nonNegativeInteger(value['createdAt'], 'createdAt')` |
| `sessionQuery` | `const` | [`packages/host/apiproxy/src/api-proxy.ts:2043`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/src/api-proxy.ts#L2043) | `const sessionQuery = ctx.get('sessionQuery')` |
| `sessionFilters` | `const` | [`packages/session-query/session-query-sqlite/src/query.ts:104`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/session-query/session-query-sqlite/src/query.ts#L104) | `const sessionFilters = materializeSessionResultFilters(request.sessionFilters ?? [])` |
| `query` | `const` | [`packages/session-query/session-query-sqlite/src/query.ts:322`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/session-query/session-query-sqlite/src/query.ts#L322) | `const query = value.trim().replace(/\s+/gu, ' ')` |
| `limit` | `const` | [`packages/session-query/session-query-sqlite/src/query.ts:370`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/session-query/session-query-sqlite/src/query.ts#L370) | `const limit = value ?? limits.defaultLimit` |
| `SessionSearchCursor` | `type` | [`packages/session-query/session-query/src/cursor.ts:6`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/session-query/session-query/src/cursor.ts#L6) | `export type SessionSearchCursor = Branded<'SessionSearchCursor'>` |
| `SessionSearchCursor` | `function` | [`packages/session-query/session-query/src/cursor.ts:13`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/session-query/session-query/src/cursor.ts#L13) | `export function SessionSearchCursor(value: string): SessionSearchCursor {` |
| `current` | `const` | [`packages/session-query/session-query/src/tracing.ts:190`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/session-query/session-query/src/tracing.ts#L190) | `const current = new Set(folded.nodes)` |
| `SessionResultFilter` | `type` | [`packages/session-query/session-query/src/types.ts:194`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/session-query/session-query/src/types.ts#L194) | `export type SessionResultFilter =` |
| `SessionEventResultFilter` | `type` | [`packages/session-query/session-query/src/types.ts:205`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/session-query/session-query/src/types.ts#L205) | `export type SessionEventResultFilter =` |
| `SessionEventSearchHit` | `interface` | [`packages/session-query/session-query/src/types.ts:270`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/session-query/session-query/src/types.ts#L270) | `export interface SessionEventSearchHit extends SessionEventRecord {` |
| `SessionSearchHit` | `interface` | [`packages/session-query/session-query/src/types.ts:276`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/session-query/session-query/src/types.ts#L276) | `export interface SessionSearchHit extends SessionRecord {` |
| `SessionPersistence` | `class` | [`packages/session/session-persistence/src/index.ts:84`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/session/session-persistence/src/index.ts#L84) | `export abstract class SessionPersistence extends Service {` |

### Tests and executable evidence

- [`packages/core/session/tests/surface.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/tests/surface.spec.ts) — A test under the owning area exercises or imports `foldSurface`.
- [`packages/host/apiproxy/tests/api-proxy-fork.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/tests/api-proxy-fork.spec.ts) — A test under the owning area exercises or imports `sessionQuery`.
- [`packages/host/apiproxy/tests/session-export.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/tests/session-export.spec.ts) — A test under the owning area exercises or imports `sessionQuery`.
- [`packages/host/apiproxy/tests/api-proxy-search.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/tests/api-proxy-search.spec.ts) — A test under the owning area exercises or imports `sessionQuery`.
- [`packages/session/session-persistence/tests/contract.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/session/session-persistence/tests/contract.ts) — A test under the owning area exercises or imports `SessionPersistence`.
- [`packages/client/ui-workspace/tests/apply.client.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-workspace/tests/apply.client.spec.ts) — A test under the owning area exercises or imports `searchSessions`.
- [`packages/session-query/session-query/tests/tracing.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/session-query/session-query/tests/tracing.spec.ts) — A test under the owning area exercises or imports `sessionQuery`. A test under the owning area exercises or imports `shadowed`.
- [`packages/session-query/session-query/tests/test-service.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/session-query/session-query/tests/test-service.ts) — A test under the owning area exercises or imports `searchSessions`. A test under the owning area exercises or imports `SessionSearchHit`.

## How to read the implementation

1. Start with [`packages/session-query/session-query/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/session-query/session-query/src/index.ts) because it has the strongest evidence link to the note.
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
- **`rust/newtype-ids`** — Represent identity domains with Rust newtypes; do not pass bare strings across session, call, turn, step, and agent boundaries.
- **`rust/provider-adapter`** — Keep provider wire quirks inside adapters; the loop and durable event vocabulary remain provider-neutral.
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

- Tags: `class/feature`, `concern/boundary`, `concern/cancellation-timeout`, `concern/compatibility`, `concern/concurrency`, `concern/discovery-routing`, `concern/evidence`, `concern/lifecycle`, `concern/ownership`, `concern/recovery`, `concern/schema-types`, `domain/llm`, `domain/observability`, `domain/session-state`, `domain/shell-terminal`, `domain/storage`, `domain/testing`, `lifecycle/implemented`, `mechanism/generation`, `mechanism/projection`, `mechanism/registry`, `mechanism/streaming`
- Aliases: `sessions`, `searchSessions`, `foldSurface`, `createdAt`, `sessionQuery`, `sessionFilters`, `query`, `limit`, `SessionSearchCursor`, `current`, `SessionResultFilter`, `SessionEventResultFilter`, `SessionEventSearchHit`, `SessionSearchHit`
- Regex: `(?i)(sessions|searchSessions|foldSurface|createdAt|sessionQuery|sessionFilters|query|limit)`

```bash
rg -n --pcre2 "(?i)(sessions|searchSessions|foldSurface|createdAt|sessionQuery|sessionFilters|query|limit)" source-deepseek-harness
rg -l --fixed-strings "class/feature" 03-agent-context
```

## Connected notes

- **`source-link`** — [0552. Unified session query service](0552-unified-session-query-service.md): The source note links to this decision directly.
- **`shares-code-with`** — [0579. Product-level TUI session resume](0579-product-level-tui-session-resume.md): Shares source implementation: `packages/session-query/session-query`, `packages/session-query/session-query/src/index.ts`.
- **`shares-code-with`** — [0565. Exact session query service](0565-exact-session-query-service.md): Shares source implementation: `packages/session-query/session-query/src/index.ts`, `packages/session-query/session-query/src/invariant.ts`.
- **`shares-code-with`** — [0197. Web past-session search](0197-web-past-session-search.md): Shares source implementation: `packages/session-query/session-query-sqlite/src/index.ts`, `packages/session-query/session-query-sqlite/src/invariant.ts`.
- **`shares-code-with`** — [0129. Session content search ships opt-in through openAt never](0129-session-content-search-ships-opt-in-through-openat-never.md): Shares source implementation: `packages/session-query/session-query-sqlite`, `packages/session-query/session-query-sqlite/src/index.ts`.
- **`shares-code-with`** — [0154. Session query relationship tracing](0154-session-query-relationship-tracing.md): Shares source implementation: `packages/session-query/session-query/src/tracing.ts`, `packages/session-query/session-query/src/types.ts`.
- **`same-design-pressure`** — [0509. Domain KV storage capability seam and the workspace entity](0509-domain-kv-storage-capability-seam-and-the-workspace-entity.md): Shares design concerns: `concern/boundary`, `concern/cancellation-timeout`, `concern/compatibility`.
- **`shares-code-with`** — [0243. Session search tools are not a shipped default](0243-session-search-tools-are-not-a-shipped-default.md): Shares source implementation: `packages/session-query/session-query-sqlite/src/index.ts`, `packages/session-query/session-query-sqlite/src/invariant.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0152-sqlite-fts5-session-search.md`.
