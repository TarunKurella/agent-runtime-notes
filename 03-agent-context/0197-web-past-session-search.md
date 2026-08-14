---
id: "dsh-note-0197"
title: "Web past-session search"
status: "implemented"
class: "feature"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/feature/2026-07-27-web-session-search.md"
implementation_evidence: "medium"
target_anchor: "read/write/patch filesystem capability"
tags:
  - "class/feature"
  - "concern/boundary"
  - "concern/cancellation-timeout"
  - "concern/compatibility"
  - "concern/concurrency"
  - "concern/evidence"
  - "concern/lifecycle"
  - "concern/ownership"
  - "concern/performance"
  - "concern/recovery"
  - "concern/schema-types"
  - "concern/simplification"
  - "domain/build-release"
  - "domain/configuration"
  - "domain/filesystem"
  - "domain/llm"
  - "domain/multi-agent"
  - "domain/observability"
  - "domain/protocols"
  - "domain/session-state"
  - "domain/storage"
  - "domain/testing"
  - "domain/ui-interaction"
  - "domain/web-retrieval"
  - "lifecycle/implemented"
  - "mechanism/generation"
  - "mechanism/policy"
  - "mechanism/projection"
  - "mechanism/streaming"
aliases:
  - "internal"
  - "ISessions"
  - "search"
  - "WorkspaceBrowser"
  - "searchSessions"
  - "list"
  - "sessionQuery"
  - "SESSION_SEARCH_RESULT_LIMIT"
  - "sessions"
  - "hasMore"
  - "@deepseek-ai/dsh-session-query-sqlite"
  - "openAt: first-search"
  - "session.search"
  - "session.list"
search_regex: "(?i)(internal|ISessions|search|WorkspaceBrowser|searchSessions|list|sessionQuery|SESSION_SEARCH_RESULT_LIMIT)"
---

# 0197. Web past-session search — implementation context

## Open this when

The Web sidebar exposes session titles and Workspace membership but cannot retrieve a past conversation from words that appear only inside its messages. Scanning histories in the browser would require attaching or loading every session, duplicate the existing indexed-search service, and make cold persisted sessions both slow and easy to omit. The product also needs a predictable failure path: an unavailable derived index must not erase title matches that the client can compute locally.

## Source decision

The shared Web/headless composition mounts @deepseek-ai/dsh-session-query-sqlite with openAt: first-search and an in-memory database. The service is ACTIVE at boot, while its node:sqlite module and connection-private handle open only on the first content query. This keeps Node 22 startup output free of SQLite's experimental warning before search is used without promising to suppress the warning when search first imports the module. Each service instance owns its index, preserving the SQLite backend's single-owner contract across parallel CLI or Web invocations without leaving process-scoped derived files behind.

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/feature/2026-07-27-web-session-search.md](../02-notes/implemented/feature/2026-07-27-web-session-search.md)
- Pinned source: [.agents/notes/implemented/feature/2026-07-27-web-session-search.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/feature/2026-07-27-web-session-search.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`packages/client/ui-workspace/README.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-workspace/README.md) | package contract and examples | The source note names this file directly. | `named-file` |
| [`packages/session-query/session-query-sqlite/README.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/session-query/session-query-sqlite/README.md) | package contract and examples | The source note names this file directly. Core file in the package named by the note: `packages/session-query/session-query-sqlite`. | `named-file, named-package-member` |
| [`packages/host/apiproxy/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/src/index.ts) | package entry point | Core file in the package named by the note: `packages/host/apiproxy`. | `named-package-member` |
| [`packages/host/apiproxy/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/host/apiproxy`. | `named-package-member` |
| [`packages/host/apiproxy/src/api-proxy.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/src/api-proxy.ts) | runtime implementation | Core file in the package named by the note: `packages/host/apiproxy`. Defines `sessionQuery`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/host/apiproxy/src/session-export.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/src/session-export.ts) | runtime implementation | Core file in the package named by the note: `packages/host/apiproxy`. Defines `sessions`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/host/apiproxy/src/api/session-search.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/src/api/session-search.ts) | runtime implementation | Core file in the package named by the note: `packages/host/apiproxy`. Defines `SESSION_SEARCH_RESULT_LIMIT`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/session-query/session-query-sqlite/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/session-query/session-query-sqlite/src/index.ts) | package entry point | Core file in the package named by the note: `packages/session-query/session-query-sqlite`. Defines `hasMore`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/session-query/session-query-sqlite/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/session-query/session-query-sqlite/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/session-query/session-query-sqlite`. | `named-package-member` |
| [`packages/host/apiproxy`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy) | package or module directory | The note names this package or capability. | `named-package` |
| [`packages/session-query/session-query-sqlite`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/packages/session-query/session-query-sqlite) | package or module directory | The note names this package or capability. | `named-package` |
| [`packages/boot/app-boot/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/boot/app-boot/src/index.ts) | package entry point | Defines `internal`, a construct named by the note. | `symbol-definition` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `internal` | `const` | [`packages/boot/app-boot/src/index.ts:498`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/boot/app-boot/src/index.ts#L498) | `const internal = this.ctx.loader.internal` |
| `ISessions` | `interface` | [`packages/client/runtime/src/client/contract/sessions.ts:26`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/runtime/src/client/contract/sessions.ts#L26) | `export interface ISessions {` |
| `search` | `const` | [`packages/client/ui-tool/src/client/tool/ToolDetails.tsx:41`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-tool/src/client/tool/ToolDetails.tsx#L41) | `const search = searchCardModel(block)` |
| `WorkspaceBrowser` | `function` | [`packages/client/ui-workspace/src/client/WorkspaceBrowser.tsx:741`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-workspace/src/client/WorkspaceBrowser.tsx#L741) | `export function WorkspaceBrowser({` |
| `searchSessions` | `const` | [`packages/client/ui-workspace/src/client/index.ts:56`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-workspace/src/client/index.ts#L56) | `const searchSessions: WorkspaceBrowserInjected['searchSessions'] = async (query, signal) => {` |
| `list` | `const` | [`packages/hooks/hook-protocol/src/merge.ts:75`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/hooks/hook-protocol/src/merge.ts#L75) | `const list = reasonsByRank.get(r) ?? []` |
| `sessionQuery` | `const` | [`packages/host/apiproxy/src/api-proxy.ts:2043`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/src/api-proxy.ts#L2043) | `const sessionQuery = ctx.get('sessionQuery')` |
| `SESSION_SEARCH_RESULT_LIMIT` | `const` | [`packages/host/apiproxy/src/api/session-search.ts:2`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/src/api/session-search.ts#L2) | `export const SESSION_SEARCH_RESULT_LIMIT = 20` |
| `sessions` | `const` | [`packages/host/apiproxy/src/session-export.ts:79`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/src/session-export.ts#L79) | `const sessions = deps.sessions` |
| `hasMore` | `const` | [`packages/session-query/session-query-sqlite/src/index.ts:948`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/session-query/session-query-sqlite/src/index.ts#L948) | `const hasMore = rows.length > limit` |

### Tests and executable evidence

- [`packages/host/apiproxy/tests/api-proxy-fork.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/tests/api-proxy-fork.spec.ts) — A test under the owning area exercises or imports `dsh-host-apiproxy`. A test under the owning area exercises or imports `sessionQuery`.
- [`packages/host/apiproxy/tests/client-handler.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/tests/client-handler.spec.ts) — A test under the owning area exercises or imports `dsh-host-apiproxy`.
- [`packages/host/apiproxy/tests/session-export.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/tests/session-export.spec.ts) — A test under the owning area exercises or imports `sessionQuery`.
- [`packages/host/apiproxy/tests/api-proxy-blank.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/tests/api-proxy-blank.spec.ts) — A test under the owning area exercises or imports `dsh-host-apiproxy`.
- [`packages/host/apiproxy/tests/api-proxy-search.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/tests/api-proxy-search.spec.ts) — A test under the owning area exercises or imports `searchSessions`. A test under the owning area exercises or imports `SESSION_QUERY_INVALID_LIMIT`.
- [`packages/host/apiproxy/tests/api-proxy-rename.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/tests/api-proxy-rename.spec.ts) — A test under the owning area exercises or imports `dsh-host-apiproxy`.
- [`packages/client/ui-workspace/tests/apply.client.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-workspace/tests/apply.client.spec.ts) — A test under the owning area exercises or imports `searchSessions`. A test under the owning area exercises or imports `WorkspaceBrowser`.
- [`packages/host/apiproxy/tests/api-proxy-question.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/tests/api-proxy-question.spec.ts) — A test under the owning area exercises or imports `dsh-host-apiproxy`.
- Source verification intent: Host tests pin request and response validation, visible-session filtering, event/surface filters, result and snippet bounds, adaptive provider limits inside the shared call budget, learned-limit stale restarts, cursor and cross-page deduplication behavior, cancellation precedence, and failure mapping. SQLite lifecycle tests pin eager activation, first-search opening and failure, shared readiness, and unopened disposal; semantic extraction and SQLite/fixture search tests pin exclusion of reasoning-only text.

## How to read the implementation

1. Start with [`packages/client/ui-workspace/README.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-workspace/README.md) because it has the strongest evidence link to the note.
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
- **`rust/tokio-cancellation`** — Thread one cancellation token through async work, close owned resources before reuse, and test cancellation at every await boundary.
- **`rust/trait-seam`** — Use a small trait at the capability boundary; keep policy and model-facing schemas outside the backend implementation.

### Smallest safe implementation slice

1. Express the semantic contract as Rust types and one owning boundary.
2. Add the smallest adapter or service needed by the existing loop; do not add a new model-facing tool unless the four-tool surface cannot express the job.
3. Record durable evidence before deriving summaries or UI state.
4. Add one happy-path test, one interruption test, and one replay/restart test where state survives a turn.
5. Keep the patch reversible until the source note's strongest failure case is proven in the Rust harness.

## Search handles

- Tags: `class/feature`, `concern/boundary`, `concern/cancellation-timeout`, `concern/compatibility`, `concern/concurrency`, `concern/evidence`, `concern/lifecycle`, `concern/ownership`, `concern/performance`, `concern/recovery`, `concern/schema-types`, `concern/simplification`, `domain/build-release`, `domain/configuration`, `domain/filesystem`, `domain/llm`, `domain/multi-agent`, `domain/observability`, `domain/protocols`, `domain/session-state`, `domain/storage`, `domain/testing`, `domain/ui-interaction`, `domain/web-retrieval`, `lifecycle/implemented`, `mechanism/generation`, `mechanism/policy`, `mechanism/projection`, `mechanism/streaming`
- Aliases: `internal`, `ISessions`, `search`, `WorkspaceBrowser`, `searchSessions`, `list`, `sessionQuery`, `SESSION_SEARCH_RESULT_LIMIT`, `sessions`, `hasMore`, `@deepseek-ai/dsh-session-query-sqlite`, `openAt: first-search`, `session.search`, `session.list`
- Regex: `(?i)(internal|ISessions|search|WorkspaceBrowser|searchSessions|list|sessionQuery|SESSION_SEARCH_RESULT_LIMIT)`

```bash
rg -n --pcre2 "(?i)(internal|ISessions|search|WorkspaceBrowser|searchSessions|list|sessionQuery|SESSION_SEARCH_RESULT_LIMIT)" source-deepseek-harness
rg -l --fixed-strings "class/feature" 03-agent-context
```

## Connected notes

- **`shares-code-with`** — [0513. Record last activity in the session index](0513-record-last-activity-in-the-session-index.md): Shares source implementation: `packages/host/apiproxy`, `packages/host/apiproxy/src/api-proxy.ts`.
- **`shares-code-with`** — [0198. Web subagent catalog and human continuation](0198-web-subagent-catalog-and-human-continuation.md): Shares source implementation: `packages/host/apiproxy/src/api-proxy.ts`, `packages/host/apiproxy/src/index.ts`.
- **`shares-code-with`** — [0070. A capability-discriminated directory-picker seam for the web-GUI host](0070-a-capability-discriminated-directory-picker-seam-for-the-web-gui-host.md): Shares source implementation: `packages/host/apiproxy/src/api-proxy.ts`, `packages/host/apiproxy/src/index.ts`.
- **`shares-code-with`** — [0375. Bound cold blank-session verification](0375-bound-cold-blank-session-verification.md): Shares source implementation: `packages/host/apiproxy`, `packages/host/apiproxy/src/api-proxy.ts`.
- **`shares-code-with`** — [0129. Session content search ships opt-in through openAt never](0129-session-content-search-ships-opt-in-through-openat-never.md): Shares source implementation: `packages/host/apiproxy/src/api-proxy.ts`, `packages/host/apiproxy/src/index.ts`.
- **`shares-code-with`** — [0205. Todo plan strip clears on the next turn](0205-todo-plan-strip-clears-on-the-next-turn.md): Shares source implementation: `packages/host/apiproxy`, `packages/host/apiproxy/src/api-proxy.ts`.
- **`shares-code-with`** — [0224. Web result card --- a structured render intent for web_search and web_fetch](0224-web-result-card-a-structured-render-intent-for-web-search-and-web-fetch.md): Shares source implementation: `packages/host/apiproxy/src/api-proxy.ts`, `packages/host/apiproxy/src/index.ts`.
- **`shares-code-with`** — [0060. dsh web config-tree boot and the web transport layering](0060-dsh-web-config-tree-boot-and-the-web-transport-layering.md): Shares source implementation: `packages/host/apiproxy/src/api-proxy.ts`, `packages/host/apiproxy/src/index.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0197-web-past-session-search.md`.
