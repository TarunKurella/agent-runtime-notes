---
id: "dsh-note-0129"
title: "Session content search ships opt-in through openAt never"
status: "implemented"
class: "architecture"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/architecture/2026-08-13-session-content-search-opt-in.md"
implementation_evidence: "medium"
target_anchor: "read/write/patch filesystem capability"
tags:
  - "class/architecture"
  - "concern/boundary"
  - "concern/recovery"
  - "concern/schema-types"
  - "domain/build-release"
  - "domain/extensions"
  - "domain/filesystem"
  - "domain/llm"
  - "domain/multi-agent"
  - "domain/observability"
  - "domain/session-state"
  - "domain/storage"
  - "domain/ui-interaction"
  - "lifecycle/implemented"
  - "mechanism/projection"
aliases:
  - "search"
  - "searchSessions"
  - "startup"
  - "sessionQuery"
  - "ApiProxyService"
  - "SessionQueryErrorCode"
  - "traceSession"
  - "path"
  - "openAt: first-search"
  - "ctx.sessionQuery.traceSession"
  - "openAt: 'never"
  - "@deepseek-ai/dsh-session-query-sqlite"
  - "searchEvents"
  - "SESSION_QUERY_SEARCH_DISABLED"
search_regex: "(?i)(search|searchSessions|startup|sessionQuery|ApiProxyService|SessionQueryErrorCode|traceSession|path)"
---

# 0129. Session content search ships opt-in through openAt never — implementation context

## Open this when

The shipped bundles mounted the SQLite session-query provider with the full-text index live (openAt: first-search), so every default deployment carried a derived FTS index and the Web sidebar offered content search. Whether a deployment wants that index --- its node:sqlite import, per-search source reconciliation, and derived storage --- is a deployment choice, and the product default is to ship without it; the model-facing search tools were already opt-in and unmounted (the not-shipped-default decision). Turning the capability off by unmounting the plugin row is not viable.

## Source decision

Content search is enforced off at the provider. openAt: 'never' is a third opening phase on @deepseek-ai/dsh-session-query-sqlite: searchSessions and searchEvents fail with the typed SESSION_QUERY_SEARCH_DISABLED code before any request normalization, node:sqlite is never imported or opened, and no source observation or reconciliation runs. Every inherited ctx.sessionQuery exact read, filter, and trace keeps working, so session export, fork Workspace inheritance, and title reads are unaffected.

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/architecture/2026-08-13-session-content-search-opt-in.md](../02-notes/implemented/architecture/2026-08-13-session-content-search-opt-in.md)
- Pinned source: [.agents/notes/implemented/architecture/2026-08-13-session-content-search-opt-in.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/architecture/2026-08-13-session-content-search-opt-in.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`packages/session-query/tool-session-query/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/session-query/tool-session-query/src/index.ts) | package entry point | Core file in the package named by the note: `packages/session-query/tool-session-query`. | `named-package-member` |
| [`packages/session-query/session-query-sqlite/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/session-query/session-query-sqlite/src/index.ts) | package entry point | Core file in the package named by the note: `packages/session-query/session-query-sqlite`. | `named-package-member` |
| [`packages/session-query/tool-session-query/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/session-query/tool-session-query/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/session-query/tool-session-query`. | `named-package-member` |
| [`packages/session-query/session-query-sqlite/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/session-query/session-query-sqlite/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/session-query/session-query-sqlite`. | `named-package-member` |
| [`packages/session-query/tool-session-query`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/packages/session-query/tool-session-query) | package or module directory | The note names this package or capability. | `named-package` |
| [`packages/session-query/session-query-sqlite`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/packages/session-query/session-query-sqlite) | package or module directory | The note names this package or capability. | `named-package` |
| [`vendor/hmr/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/hmr/src/index.ts) | package entry point | Defines `path`, a construct named by the note. | `symbol-definition` |
| [`packages/host/apiproxy/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/src/index.ts) | package entry point | Defines `ApiProxyService`, a construct named by the note. | `symbol-definition` |
| [`packages/core/agent-loop/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent-loop/src/index.ts) | package entry point | Defines `startup`, a construct named by the note. | `symbol-definition` |
| [`packages/host/apiproxy/src/api-proxy.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/src/api-proxy.ts) | runtime implementation | Defines `sessionQuery`, a construct named by the note. | `symbol-definition` |
| [`packages/client/ui-workspace/src/client/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-workspace/src/client/index.ts) | package entry point | Defines `searchSessions`, a construct named by the note. | `symbol-definition` |
| [`packages/session-query/session-query/src/config.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/session-query/session-query/src/config.ts) | runtime implementation | Defines `SessionQueryErrorCode`, a construct named by the note. | `symbol-definition` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `search` | `const` | [`packages/client/ui-tool/src/client/tool/ToolDetails.tsx:41`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-tool/src/client/tool/ToolDetails.tsx#L41) | `const search = searchCardModel(block)` |
| `searchSessions` | `const` | [`packages/client/ui-workspace/src/client/index.ts:56`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-workspace/src/client/index.ts#L56) | `const searchSessions: WorkspaceBrowserInjected['searchSessions'] = async (query, signal) => {` |
| `startup` | `const` | [`packages/core/agent-loop/src/index.ts:363`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent-loop/src/index.ts#L363) | `const startup = this.restoreOrCreateConfigured(ctx, persistence, configuredId, options, meta).catch((error: unknown) => {` |
| `sessionQuery` | `const` | [`packages/host/apiproxy/src/api-proxy.ts:2043`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/src/api-proxy.ts#L2043) | `const sessionQuery = ctx.get('sessionQuery')` |
| `ApiProxyService` | `class` | [`packages/host/apiproxy/src/index.ts:69`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/src/index.ts#L69) | `export class ApiProxyService extends Service implements ApiProxy {` |
| `SessionQueryErrorCode` | `type` | [`packages/session-query/session-query/src/config.ts:20`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/session-query/session-query/src/config.ts#L20) | `export type SessionQueryErrorCode =` |
| `traceSession` | `function` | [`packages/session-query/session-query/src/tracing.ts:113`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/session-query/session-query/src/tracing.ts#L113) | `export function traceSession(` |
| `path` | `const` | [`vendor/hmr/src/index.ts:514`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/hmr/src/index.ts#L514) | `const path = relative(this.baseDir, fileURLToPath(filename))` |

### Tests and executable evidence

- [`packages/host/apiproxy/tests/session-export.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/tests/session-export.spec.ts) — A test under the owning area exercises or imports `ApiProxyService`. A test under the owning area exercises or imports `sessionQuery`.
- [`packages/host/apiproxy/tests/api-proxy-fork.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/tests/api-proxy-fork.spec.ts) — A test under the owning area exercises or imports `sessionQuery`.
- [`packages/host/apiproxy/tests/api-proxy-search.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/tests/api-proxy-search.spec.ts) — A test under the owning area exercises or imports `sessionQuery`.
- [`packages/client/ui-workspace/tests/apply.client.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-workspace/tests/apply.client.spec.ts) — A test under the owning area exercises or imports `searchSessions`.
- [`packages/session-query/session-query/tests/tracing.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/session-query/session-query/tests/tracing.spec.ts) — A test under the owning area exercises or imports `traceSession`. A test under the owning area exercises or imports `SessionQueryErrorCode`.
- [`packages/session-query/session-query-sqlite/tests/query.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/session-query/session-query-sqlite/tests/query.spec.ts) — A test under the owning area exercises or imports `SessionQueryErrorCode`.
- [`packages/session-query/session-query-sqlite/tests/sqlite.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/session-query/session-query-sqlite/tests/sqlite.spec.ts) — A test under the owning area exercises or imports `sessionQuery`. A test under the owning area exercises or imports `traceSession`.
- [`packages/session-query/session-query/tests/session-query.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/session-query/session-query/tests/session-query.spec.ts) — A test under the owning area exercises or imports `traceSession`. A test under the owning area exercises or imports `SessionQueryErrorCode`.

## How to read the implementation

1. Start with [`packages/session-query/tool-session-query/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/session-query/tool-session-query/src/index.ts) because it has the strongest evidence link to the note.
2. Read public types and entry points before implementation details. Write down the contract in plain language.
3. Follow the named constructs into their callers and tests. Confirm which component can actually enforce the rule.
4. Reconstruct one failure path: cancellation, crash, retry, stale state, partial write, or incompatible provider input—whichever the note is protecting against.
5. Only then map the contract into the current Rust repository. Target paths must come from that repository, never from guesswork in this wiki.

## Rust harness port

**Likely integration point:** read/write/patch filesystem capability.

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
- **`rust/trait-seam`** — Use a small trait at the capability boundary; keep policy and model-facing schemas outside the backend implementation.

### Smallest safe implementation slice

1. Express the semantic contract as Rust types and one owning boundary.
2. Add the smallest adapter or service needed by the existing loop; do not add a new model-facing tool unless the four-tool surface cannot express the job.
3. Record durable evidence before deriving summaries or UI state.
4. Add one happy-path test, one interruption test, and one replay/restart test where state survives a turn.
5. Keep the patch reversible until the source note's strongest failure case is proven in the Rust harness.

## Search handles

- Tags: `class/architecture`, `concern/boundary`, `concern/recovery`, `concern/schema-types`, `domain/build-release`, `domain/extensions`, `domain/filesystem`, `domain/llm`, `domain/multi-agent`, `domain/observability`, `domain/session-state`, `domain/storage`, `domain/ui-interaction`, `lifecycle/implemented`, `mechanism/projection`
- Aliases: `search`, `searchSessions`, `startup`, `sessionQuery`, `ApiProxyService`, `SessionQueryErrorCode`, `traceSession`, `path`, `openAt: first-search`, `ctx.sessionQuery.traceSession`, `openAt: 'never`, `@deepseek-ai/dsh-session-query-sqlite`, `searchEvents`, `SESSION_QUERY_SEARCH_DISABLED`
- Regex: `(?i)(search|searchSessions|startup|sessionQuery|ApiProxyService|SessionQueryErrorCode|traceSession|path)`

```bash
rg -n --pcre2 "(?i)(search|searchSessions|startup|sessionQuery|ApiProxyService|SessionQueryErrorCode|traceSession|path)" source-deepseek-harness
rg -l --fixed-strings "class/architecture" 03-agent-context
```

## Connected notes

- **`source-link`** — [0243. Session search tools are not a shipped default](0243-session-search-tools-are-not-a-shipped-default.md): The source note links to this decision directly.
- **`shares-code-with`** — [0197. Web past-session search](0197-web-past-session-search.md): Shares source implementation: `packages/host/apiproxy/src/api-proxy.ts`, `packages/host/apiproxy/src/index.ts`.
- **`shares-code-with`** — [0180. Model-facing session query tools](0180-model-facing-session-query-tools.md): Shares source implementation: `packages/session-query/tool-session-query`, `packages/session-query/tool-session-query/src/index.ts`.
- **`shares-code-with`** — [0152. SQLite FTS5 session search](0152-sqlite-fts5-session-search.md): Shares source implementation: `packages/session-query/session-query-sqlite`, `packages/session-query/session-query-sqlite/src/index.ts`.
- **`shares-code-with`** — [0198. Web subagent catalog and human continuation](0198-web-subagent-catalog-and-human-continuation.md): Shares source implementation: `packages/host/apiproxy/src/api-proxy.ts`, `packages/host/apiproxy/src/index.ts`.
- **`shares-code-with`** — [0127. Trajectory assembly from registered Conversation Contexts](0127-trajectory-assembly-from-registered-conversation-contexts.md): Shares source implementation: `packages/host/apiproxy/src/api-proxy.ts`, `vendor/hmr/src/index.ts`.
- **`shares-code-with`** — [0070. A capability-discriminated directory-picker seam for the web-GUI host](0070-a-capability-discriminated-directory-picker-seam-for-the-web-gui-host.md): Shares source implementation: `packages/host/apiproxy/src/api-proxy.ts`, `packages/host/apiproxy/src/index.ts`.
- **`shares-code-with`** — [0224. Web result card --- a structured render intent for web_search and web_fetch](0224-web-result-card-a-structured-render-intent-for-web-search-and-web-fetch.md): Shares source implementation: `packages/host/apiproxy/src/api-proxy.ts`, `packages/host/apiproxy/src/index.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0129-session-content-search-ships-opt-in-through-openat-never.md`.
