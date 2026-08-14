---
id: "dsh-note-0224"
title: "Web result card --- a structured render intent for web_search and web_fetch"
status: "implemented"
class: "feature"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/feature/2026-07-30-web-result-card.md"
implementation_evidence: "high"
target_anchor: "read/write/patch filesystem capability"
tags:
  - "class/feature"
  - "concern/boundary"
  - "concern/discovery-routing"
  - "concern/evidence"
  - "concern/ownership"
  - "concern/recovery"
  - "concern/schema-types"
  - "domain/build-release"
  - "domain/filesystem"
  - "domain/llm"
  - "domain/session-state"
  - "domain/shell-terminal"
  - "domain/storage"
  - "domain/testing"
  - "domain/ui-interaction"
  - "domain/web-retrieval"
  - "lifecycle/implemented"
  - "mechanism/event-sourcing"
  - "mechanism/policy"
  - "mechanism/projection"
  - "mechanism/registry"
aliases:
  - "web"
  - "card"
  - "ToolResultView"
  - "WebSource"
  - "query"
  - "snippet"
  - "title"
  - "url"
  - "terminal"
  - "render"
  - "renderFetchOutput"
  - "truncated"
  - "fetchMetaFromValue"
  - "fetchMetaFromResult"
search_regex: "(?i)(card|ToolResultView|WebSource|query|snippet|title|terminal|render)"
---

# 0224. Web result card --- a structured render intent for web_search and web_fetch — implementation context

## Open this when

The web_search and web_fetch tools each declared a generic pending card (presentCall, kind: 'search'/'fetch') but no presentResult, so a completed web call reached a UI only as the model-facing render text. For a web frontend that wants to render a citation list or a fetch summary, that text is lossy: web_search's render collapses each source's title, snippet, and publishedAt into one free-text markdown line labelled by title OR hostname (formatSearchOutput in packages/web/tool-web/src/search.ts), so reparsing the render cannot recover the per-source fields; and web_fetch's render carries url and statusCode.

## Source decision

One tag with a kind discriminant, not two tags. Both calls are web retrieval and a web frontend renders them with one component family (a retrieval card whose body differs by kind), so a shared card keeps every card consumer's switch to one added arm and lets the frontend branch on kind inside it. Two tags would force every present and future consumer to add two arms for what is one visual family. The kind values match the two tools' existing generic call-view kinds, so a call and its result read as the same category. presentationMeta carries what render text cannot.

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/feature/2026-07-30-web-result-card.md](../02-notes/implemented/feature/2026-07-30-web-result-card.md)
- Pinned source: [.agents/notes/implemented/feature/2026-07-30-web-result-card.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/feature/2026-07-30-web-result-card.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`packages/fs/tool-fs/src/diff.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/tool-fs/src/diff.ts) | runtime implementation | The source note names this file directly. | `named-file` |
| [`packages/web/tool-web/src/search.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/web/tool-web/src/search.ts) | runtime implementation | The source note names this file directly. Defines `formatSearchOutput`, a construct named by the note. | `named-file, symbol-definition` |
| [`packages/core/tools/src/presentation.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/presentation.ts) | runtime implementation | The source note names this file directly. Defines `ToolResultView`, a construct named by the note. | `named-file, symbol-definition` |
| [`packages/host/apiproxy/src/api/sessions.schema.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/src/api/sessions.schema.ts) | runtime implementation | The source note names this file directly. | `named-file` |
| [`packages/host/apiproxy/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/src/index.ts) | package entry point | Core file in the package named by the note: `packages/host/apiproxy`. | `named-package-member` |
| [`packages/host/apiproxy/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/host/apiproxy`. | `named-package-member` |
| [`packages/host/apiproxy/src/api-proxy.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/src/api-proxy.ts) | runtime implementation | Core file in the package named by the note: `packages/host/apiproxy`. Defines `title`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/terminal/terminal/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/terminal/terminal/src/index.ts) | package entry point | Core file in the package named by the note: `packages/terminal/terminal`. | `named-package-member` |
| [`packages/terminal/terminal/src/types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/terminal/terminal/src/types.ts) | public types and contract | Core file in the package named by the note: `packages/terminal/terminal`. | `named-package-member` |
| [`packages/host/apiproxy/src/fetch/handler.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/src/fetch/handler.ts) | runtime implementation | Core file in the package named by the note: `packages/host/apiproxy`. Defines `url`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/terminal/terminal/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/terminal/terminal/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/terminal/terminal`. | `named-package-member` |
| [`apps/web`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/apps/web) | package or module directory | The note names this package or capability. | `named-package` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `web` | `const` | [`apps/cli/src/args.ts:156`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/cli/src/args.ts#L156) | `const web = program.command('web').description('boot the web profile (alias of --profile web); the web app\'s own flags follow')` |
| `card` | `const` | [`packages/client/ui-primitives/src/HoverCard.tsx:140`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-primitives/src/HoverCard.tsx#L140) | `const card = cardRef.current` |
| `ToolResultView` | `type` | [`packages/core/tools/src/presentation.ts:140`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/presentation.ts#L140) | `export type ToolResultView = GenericResultView \| TerminalResultView \| DiffResultView \| SearchResultView \| ReadResultView \| WebResultView` |
| `WebSource` | `interface` | [`packages/core/tools/src/presentation.ts:319`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/presentation.ts#L319) | `export interface WebSource {` |
| `query` | `const` | [`packages/examples/acp-demo/src/index.ts:134`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/examples/acp-demo/src/index.ts#L134) | `const query = ctx.plugin(SqliteSessionQueryEngine, { path: join(persistenceRoot, 'session-query.db') })` |
| `snippet` | `const` | [`packages/host/apiproxy/src/api-proxy.ts:2128`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/src/api-proxy.ts#L2128) | `const snippet = truncateUnicodeCodePoints(` |
| `title` | `const` | [`packages/host/apiproxy/src/api-proxy.ts:2831`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/src/api-proxy.ts#L2831) | `const title = payload.title.trim()` |
| `url` | `const` | [`packages/host/apiproxy/src/fetch/handler.ts:249`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/src/fetch/handler.ts#L249) | `const url = new URL(req.url)` |
| `terminal` | `const` | [`packages/jobs/jobs/src/invariant.ts:30`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/jobs/jobs/src/invariant.ts#L30) | `const terminal = TERMINAL_STATUSES.has(snapshot.status)` |
| `render` | `const` | [`packages/llm/llm/src/error.ts:118`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/src/error.ts#L118) | `const render = (current: unknown): string => {` |
| `renderFetchOutput` | `function` | [`packages/web/tool-web/src/fetch.ts:284`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/web/tool-web/src/fetch.ts#L284) | `function renderFetchOutput(result: WebFetchResult, maxOutputChars: number): RenderedFetch {` |
| `truncated` | `const` | [`packages/web/tool-web/src/fetch.ts:314`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/web/tool-web/src/fetch.ts#L314) | `const truncated = result.truncated \|\| rendered.sourceTruncated \|\| prefix.length > maxOutputChars` |
| `fetchMetaFromValue` | `function` | [`packages/web/tool-web/src/fetch.ts:374`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/web/tool-web/src/fetch.ts#L374) | `export function fetchMetaFromValue(value: WebFetchResult, maxOutputChars: number): JsonValue {` |
| `fetchMetaFromResult` | `function` | [`packages/web/tool-web/src/fetch.ts:386`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/web/tool-web/src/fetch.ts#L386) | `export function fetchMetaFromResult(meta: unknown): WebFetchMeta \| undefined {` |
| `presentFetchResult` | `function` | [`packages/web/tool-web/src/fetch.ts:405`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/web/tool-web/src/fetch.ts#L405) | `export function presentFetchResult(args: { url: string }, result: ToolResult): WebFetchResultView \| undefined {` |
| `formatSearchOutput` | `function` | [`packages/web/tool-web/src/search.ts:54`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/web/tool-web/src/search.ts#L54) | `export function formatSearchOutput(result: WebSearchResult): string {` |

### Tests and executable evidence

- [`packages/web/tool-web/tests/tool-web.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/web/tool-web/tests/tool-web.spec.ts) — The source note names this file directly. A test under the owning area exercises or imports `formatSearchOutput`.
- [`apps/web/tests/smoke-real.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/smoke-real.e2e.ts) — A test under the owning area exercises or imports `web_search`. A test under the owning area exercises or imports `web_fetch`.
- [`apps/web/tests/assembled-boot.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/assembled-boot.ts) — A test under the owning area exercises or imports `title`.
- [`apps/web/tests/plan-review.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/plan-review.e2e.ts) — A test under the owning area exercises or imports `card`.
- [`apps/web/tests/pwa-manifest.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/pwa-manifest.e2e.ts) — A test under the owning area exercises or imports `query`.
- [`apps/web/tests/todo-row.snapshot.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/todo-row.snapshot.ts) — A test under the owning area exercises or imports `title`.
- [`apps/web/tests/queue-actions.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/queue-actions.e2e.ts) — A test under the owning area exercises or imports `card`.
- [`apps/web/tests/pwsh-terminal.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/pwsh-terminal.e2e.ts) — A test under the owning area exercises or imports `card`.
- Source verification intent: packages/web/tool-web/tests/tool-web.spec.ts covers, per-file to the 100% gate: searchMetaFromValue/fetchMetaFromValue projection including omission of absent optional fields, and the fetch truncated projection agreeing with the render footer both when only the output cap cut the body and when nothing did; searchMetaFromResult/fetchMetaFromResult narrowing with a round-trip and every malformed-shape rejection (non-object, wrong field types, a malformed source entry) plus the empty-source-list accept; presentSearchResult/presentFetchResult typed views including the args-derived title, the absence of a content.

## How to read the implementation

1. Start with [`packages/fs/tool-fs/src/diff.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/tool-fs/src/diff.ts) because it has the strongest evidence link to the note.
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

- Tags: `class/feature`, `concern/boundary`, `concern/discovery-routing`, `concern/evidence`, `concern/ownership`, `concern/recovery`, `concern/schema-types`, `domain/build-release`, `domain/filesystem`, `domain/llm`, `domain/session-state`, `domain/shell-terminal`, `domain/storage`, `domain/testing`, `domain/ui-interaction`, `domain/web-retrieval`, `lifecycle/implemented`, `mechanism/event-sourcing`, `mechanism/policy`, `mechanism/projection`, `mechanism/registry`
- Aliases: `web`, `card`, `ToolResultView`, `WebSource`, `query`, `snippet`, `title`, `url`, `terminal`, `render`, `renderFetchOutput`, `truncated`, `fetchMetaFromValue`, `fetchMetaFromResult`
- Regex: `(?i)(card|ToolResultView|WebSource|query|snippet|title|terminal|render)`

```bash
rg -n --pcre2 "(?i)(card|ToolResultView|WebSource|query|snippet|title|terminal|render)" source-deepseek-harness
rg -l --fixed-strings "class/feature" 03-agent-context
```

## Connected notes

- **`source-link`** — [0023. Tagged render-intent union for tool-call presentation](0023-tagged-render-intent-union-for-tool-call-presentation.md): The source note links to this decision directly.
- **`source-link`** — [0207. Web terminal card --- the bash render intent reaches the browser](0207-web-terminal-card-the-bash-render-intent-reaches-the-browser.md): The source note links to this decision directly.
- **`source-link`** — [0223. Web result card frontend --- rendering the web render intent in the browser](0223-web-result-card-frontend-rendering-the-web-render-intent-in-the-browser.md): The source note links to this decision directly.
- **`shares-code-with`** — [0218. Web diff card --- the write/edit render intent reaches the browser](0218-web-diff-card-the-write-edit-render-intent-reaches-the-browser.md): Shares source implementation: `packages/fs/tool-fs/src/diff.ts`, `packages/terminal/terminal/src/index.ts`.
- **`shares-code-with`** — [0198. Web subagent catalog and human continuation](0198-web-subagent-catalog-and-human-continuation.md): Shares source implementation: `packages/host/apiproxy/src/api-proxy.ts`, `packages/host/apiproxy/src/index.ts`.
- **`shares-code-with`** — [0070. A capability-discriminated directory-picker seam for the web-GUI host](0070-a-capability-discriminated-directory-picker-seam-for-the-web-gui-host.md): Shares source implementation: `packages/host/apiproxy/src/api-proxy.ts`, `packages/host/apiproxy/src/index.ts`.
- **`shares-code-with`** — [0197. Web past-session search](0197-web-past-session-search.md): Shares source implementation: `packages/host/apiproxy/src/api-proxy.ts`, `packages/host/apiproxy/src/index.ts`.
- **`shares-code-with`** — [0225. Web search card --- the grep and glob render intent reaches the browser](0225-web-search-card-the-grep-and-glob-render-intent-reaches-the-browser.md): Shares source implementation: `packages/terminal/terminal/src/index.ts`, `packages/terminal/terminal/src/types.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0224-web-result-card-a-structured-render-intent-for-web-search-and-web-fetch.md`.
