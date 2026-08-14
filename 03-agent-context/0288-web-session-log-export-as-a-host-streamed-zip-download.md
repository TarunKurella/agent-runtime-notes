---
id: "dsh-note-0288"
title: "Web session-log export as a host-streamed ZIP download"
status: "implemented"
class: "feature"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/feature/2026-08-10-web-session-log-export.md"
implementation_evidence: "medium"
target_anchor: "append-only event and session store"
tags:
  - "class/feature"
  - "concern/boundary"
  - "concern/cancellation-timeout"
  - "concern/compatibility"
  - "concern/concurrency"
  - "concern/evidence"
  - "concern/human-control"
  - "concern/ownership"
  - "concern/performance"
  - "concern/schema-types"
  - "concern/trust"
  - "domain/build-release"
  - "domain/extensions"
  - "domain/session-state"
  - "domain/storage"
  - "domain/testing"
  - "domain/ui-interaction"
  - "domain/web-retrieval"
  - "lifecycle/implemented"
  - "mechanism/streaming"
aliases:
  - "flush"
  - "log"
  - "sessionExportCompressionLevel"
  - "IApiClient"
  - "toFetchHandler"
  - "jsonl"
  - "GET /api/session.export?sessionId=…&includeDescendants=true"
  - "readRaw"
  - "session.jsonl"
  - "subagents/<id>/session.jsonl"
  - "ZipDeflate"
  - "/api"
  - "ApiProxy.downloads.sessionLog"
  - "sessionLog"
search_regex: "(?i)(flush|sessionExportCompressionLevel|IApiClient|toFetchHandler|jsonl|GET[- ]/api/session\\.export\\?sessionId=…\\&includeDescendants=true|readRaw|session\\.jsonl)"
---

# 0288. Web session-log export as a host-streamed ZIP download — implementation context

## Open this when

The Trajectory view had no way to hand a debugging artifact to a human: the raw session log lived on disk and in the host, the client history face served folded projections (not raw entries), and a session with subagents spans many independent session logs. A bug report needs the complete raw log of the whole tree, in a shape that survives being emailed around.

## Source decision

The export is a host-only download, not an RPC: GET /api/session.export?sessionId=…&includeDescendants=true streams one ZIP attachment. Every file is a session's stored artifact text verbatim: readRaw on the persistence service reads the backend's own durable bytes (the JSONL backend decodes its physical zstd frames, or returns plaintext) --- never a reconstruction from parsed events, so packed-chunk rows, key order, and line breaks survive byte-for-byte --- under its original base name (session.jsonl at the root, subagents//session.jsonl for descendants).

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/feature/2026-08-10-web-session-log-export.md](../02-notes/implemented/feature/2026-08-10-web-session-log-export.md)
- Pinned source: [.agents/notes/implemented/feature/2026-08-10-web-session-log-export.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/feature/2026-08-10-web-session-log-export.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`scripts/gen-doc-graphs.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/gen-doc-graphs.ts) | repository automation | Defines `jsonl`, a construct named by the note. | `symbol-definition` |
| [`packages/typert/loader/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/typert/loader/src/index.ts) | package entry point | Defines `flush`, a construct named by the note. | `symbol-definition` |
| [`packages/boot/app-boot/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/boot/app-boot/src/index.ts) | package entry point | Defines `flush`, a construct named by the note. | `symbol-definition` |
| [`packages/host/apiproxy/src/api-proxy.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/src/api-proxy.ts) | runtime implementation | Defines `sessionExportCompressionLevel`, a construct named by the note. | `symbol-definition` |
| [`packages/core/session/src/chunk-rows.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/chunk-rows.ts) | runtime implementation | Defines `flush`, a construct named by the note. | `symbol-definition` |
| [`packages/host/apiproxy/src/fetch/client.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/src/fetch/client.ts) | runtime implementation | Defines `IApiClient`, a construct named by the note. | `symbol-definition` |
| [`packages/host/apiproxy/src/fetch/handler.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/src/fetch/handler.ts) | runtime implementation | Defines `toFetchHandler`, a construct named by the note. | `symbol-definition` |
| [`packages/test-support/acp-snapshot/src/suite.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/test-support/acp-snapshot/src/suite.ts) | runtime implementation | Defines `log`, a construct named by the note. | `symbol-definition` |
| [`packages/client/connection/src/client/fixture.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/connection/src/client/fixture.ts) | runtime implementation | Defines `log`, a construct named by the note. Defines `flush`, a construct named by the note. | `symbol-definition` |
| [`packages/client/runtime/src/client/contract/store.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/runtime/src/client/contract/store.ts) | runtime implementation | Defines `flush`, a construct named by the note. | `symbol-definition` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `flush` | `const` | [`packages/boot/app-boot/src/index.ts:452`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/boot/app-boot/src/index.ts#L452) | `const flush = (): void => {` |
| `flush` | `const` | [`packages/client/connection/src/client/fixture.ts:1264`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/connection/src/client/fixture.ts#L1264) | `const flush = (end: number): void => {` |
| `log` | `let` | [`packages/client/connection/src/client/fixture.ts:1674`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/connection/src/client/fixture.ts#L1674) | `let log = logs.get(id)` |
| `log` | `const` | [`packages/client/connection/src/client/fixture.ts:1682`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/connection/src/client/fixture.ts#L1682) | `const log = logOf(id)` |
| `log` | `const` | [`packages/client/connection/src/client/fixture.ts:1699`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/connection/src/client/fixture.ts#L1699) | `const log = logOf(id)` |
| `flush` | `const` | [`packages/client/runtime/src/client/contract/store.ts:97`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/runtime/src/client/contract/store.ts#L97) | `const flush = rafBatch(() => { for (const fn of [...listeners]) fn() })` |
| `flush` | `const` | [`packages/core/session/src/chunk-rows.ts:196`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/chunk-rows.ts#L196) | `const flush = (): void => {` |
| `sessionExportCompressionLevel` | `const` | [`packages/host/apiproxy/src/api-proxy.ts:1107`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/src/api-proxy.ts#L1107) | `const sessionExportCompressionLevel = defaults.sessionExportCompressionLevel` |
| `IApiClient` | `interface` | [`packages/host/apiproxy/src/fetch/client.ts:87`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/src/fetch/client.ts#L87) | `export interface IApiClient {` |
| `toFetchHandler` | `function` | [`packages/host/apiproxy/src/fetch/handler.ts:243`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/src/fetch/handler.ts#L243) | `export function toFetchHandler(api: ApiProxy): { fetch: typeof fetch } {` |
| `log` | `const` | [`packages/test-support/acp-snapshot/src/suite.ts:1298`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/test-support/acp-snapshot/src/suite.ts#L1298) | `const log = result.sessionLogs[index]` |
| `log` | `const` | [`packages/test-support/acp-snapshot/src/suite.ts:1310`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/test-support/acp-snapshot/src/suite.ts#L1310) | `const log = result.sessionLogs[index]` |
| `flush` | `const` | [`packages/typert/loader/src/index.ts:392`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/typert/loader/src/index.ts#L392) | `const flush = (onError: (error: Error) => void): Promise<void>[] => {` |
| `jsonl` | `const` | [`scripts/gen-doc-graphs.ts:731`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/gen-doc-graphs.ts#L731) | `const jsonl = nodeId('bundle', 'jsonl')` |

### Tests and executable evidence

- [`apps/web/tests/scaffold.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/scaffold.ts) — A test under the owning area exercises or imports `jsonl`.
- [`scripts/change-scope.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/change-scope.spec.ts) — A test under the owning area exercises or imports `HEAD`.
- [`apps/web/tests/steering.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/steering.e2e.ts) — A test under the owning area exercises or imports `jsonl`.
- [`apps/web/tests/plan-review.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/plan-review.e2e.ts) — A test under the owning area exercises or imports `jsonl`.
- [`apps/web/tests/workflow-run.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/workflow-run.e2e.ts) — A test under the owning area exercises or imports `jsonl`.
- [`apps/web/tests/queue-actions.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/queue-actions.e2e.ts) — A test under the owning area exercises or imports `jsonl`.
- [`apps/web/tests/pwsh-terminal.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/pwsh-terminal.e2e.ts) — A test under the owning area exercises or imports `jsonl`.
- [`apps/web/tests/seeded-history.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/seeded-history.e2e.ts) — A test under the owning area exercises or imports `jsonl`.

## How to read the implementation

1. Start with [`scripts/gen-doc-graphs.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/gen-doc-graphs.ts) because it has the strongest evidence link to the note.
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
- **`rust/newtype-ids`** — Represent identity domains with Rust newtypes; do not pass bare strings across session, call, turn, step, and agent boundaries.
- **`rust/process-discipline`** — Treat this as repository or release policy unless it changes a runtime contract. Do not add runtime machinery for a process-only lesson.
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

- Tags: `class/feature`, `concern/boundary`, `concern/cancellation-timeout`, `concern/compatibility`, `concern/concurrency`, `concern/evidence`, `concern/human-control`, `concern/ownership`, `concern/performance`, `concern/schema-types`, `concern/trust`, `domain/build-release`, `domain/extensions`, `domain/session-state`, `domain/storage`, `domain/testing`, `domain/ui-interaction`, `domain/web-retrieval`, `lifecycle/implemented`, `mechanism/streaming`
- Aliases: `flush`, `log`, `sessionExportCompressionLevel`, `IApiClient`, `toFetchHandler`, `jsonl`, `GET /api/session.export?sessionId=…&includeDescendants=true`, `readRaw`, `session.jsonl`, `subagents/<id>/session.jsonl`, `ZipDeflate`, `/api`, `ApiProxy.downloads.sessionLog`, `sessionLog`
- Regex: `(?i)(flush|sessionExportCompressionLevel|IApiClient|toFetchHandler|jsonl|GET[- ]/api/session\.export\?sessionId=…\&includeDescendants=true|readRaw|session\.jsonl)`

```bash
rg -n --pcre2 "(?i)(flush|sessionExportCompressionLevel|IApiClient|toFetchHandler|jsonl|GET[- ]/api/session\\.export\\?sessionId=\u2026\\&includeDescendants=true|readRaw|session\\.jsonl)" source-deepseek-harness
rg -l --fixed-strings "class/feature" 03-agent-context
```

## Connected notes

- **`source-link`** — [0295. Web `/export` shares the streamed Session ZIP download](0295-web-export-shares-the-streamed-session-zip-download.md): The source note links to this decision directly.
- **`shares-code-with`** — [0110. Bounded session persistence write batching](0110-bounded-session-persistence-write-batching.md): Shares source implementation: `packages/client/connection/src/client/fixture.ts`, `packages/core/session/src/chunk-rows.ts`.
- **`shares-code-with`** — [0194. Trajectory inspection ledger](0194-trajectory-inspection-ledger.md): Shares source implementation: `packages/client/connection/src/client/fixture.ts`, `packages/host/apiproxy/src/api-proxy.ts`.
- **`shares-code-with`** — [0407. CI failover runbook --- hosted pools → in-house pool](0407-ci-failover-runbook-hosted-pools-in-house-pool.md): Shares source implementation: `packages/client/connection/src/client/fixture.ts`, `packages/host/apiproxy/src/api-proxy.ts`.
- **`shares-code-with`** — [0375. Bound cold blank-session verification](0375-bound-cold-blank-session-verification.md): Shares source implementation: `packages/boot/app-boot/src/index.ts`, `packages/host/apiproxy/src/api-proxy.ts`.
- **`shares-code-with`** — [0513. Record last activity in the session index](0513-record-last-activity-in-the-session-index.md): Shares source implementation: `packages/boot/app-boot/src/index.ts`, `packages/host/apiproxy/src/api-proxy.ts`.
- **`shares-code-with`** — [0197. Web past-session search](0197-web-past-session-search.md): Shares source implementation: `packages/host/apiproxy/src/api-proxy.ts`.
- **`shares-code-with`** — [0070. A capability-discriminated directory-picker seam for the web-GUI host](0070-a-capability-discriminated-directory-picker-seam-for-the-web-gui-host.md): Shares source implementation: `packages/host/apiproxy/src/api-proxy.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0288-web-session-log-export-as-a-host-streamed-zip-download.md`.
