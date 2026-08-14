---
id: "dsh-note-0243"
title: "Session search tools are not a shipped default"
status: "implemented"
class: "feature"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/feature/2026-08-02-session-search-not-shipped-default.md"
implementation_evidence: "high"
target_anchor: "Rust provider-adapter boundary"
tags:
  - "class/feature"
  - "concern/boundary"
  - "concern/cancellation-timeout"
  - "concern/performance"
  - "concern/recovery"
  - "concern/simplification"
  - "domain/build-release"
  - "domain/configuration"
  - "domain/context"
  - "domain/extensions"
  - "domain/llm"
  - "domain/protocols"
  - "domain/session-state"
  - "domain/storage"
  - "domain/ui-interaction"
  - "lifecycle/implemented"
  - "mechanism/projection"
aliases:
  - "sessionQuery"
  - "tool-session-query"
  - "cordis.patch.yml"
  - "session_search"
  - "session_event_search"
  - "session_trace"
  - "session_event_trace"
  - "session_event_read"
  - "@deepseek-ai/dsh-tool-session-query"
  - "session-query.cordis.yml"
  - "ctx.sessionQuery"
  - "session-query-sqlite"
  - "session-reference"
  - "/resume"
search_regex: "(?i)(sessionQuery|tool\\-session\\-query|cordis\\.patch\\.yml|session_search|session_event_search|session_trace|session_event_trace|session_event_read)"
---

# 0243. Session search tools are not a shipped default — implementation context

## Open this when

The shipped-roster decision made tool-session-query a default row of the shared cordis.patch.yml, so the shipped TUI and Web surfaces put the five session-search tools (session_search, session_event_search, session_trace, session_event_trace, session_event_read) in front of the model. That contradicted the model-facing session-query-tools decision, whose opt-in stance the package README recorded as "shipped host compositions do not mount it by default". The default also shipped a prompt section teaching a prior-work search workflow that no user had asked for.

## Source decision

The shipped TUI, Web, and headless surfaces do not mount @deepseek-ai/dsh-tool-session-query, and no shipped agent preset carries it. The consumer stays opt-in exactly as the model-facing-session-query-tools note describes: the ACP example's session-query.cordis.yml and its snapshot counterpart remain the mounted reference, and a custom composition can mount the package with the timeout and spill policies. The ctx.sessionQuery service itself stays mounted.

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/feature/2026-08-02-session-search-not-shipped-default.md](../02-notes/implemented/feature/2026-08-02-session-search-not-shipped-default.md)
- Pinned source: [.agents/notes/implemented/feature/2026-08-02-session-search-not-shipped-default.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/feature/2026-08-02-session-search-not-shipped-default.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`packages/bundle/base/cordis.patch.yml`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/bundle/base/cordis.patch.yml) | composition and configuration | The source note names this file directly. | `named-file` |
| [`examples/acp-agent/session-query.cordis.yml`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/examples/acp-agent/session-query.cordis.yml) | composition and configuration | The source note names this file directly. | `named-file` |
| [`packages/context/session-reference/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/context/session-reference/src/index.ts) | package entry point | Core file in the package named by the note: `packages/context/session-reference`. | `named-package-member` |
| [`packages/context/session-reference/src/types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/context/session-reference/src/types.ts) | public types and contract | Core file in the package named by the note: `packages/context/session-reference`. | `named-package-member` |
| [`packages/context/session-reference/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/context/session-reference/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/context/session-reference`. | `named-package-member` |
| [`packages/session-query/tool-session-query/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/session-query/tool-session-query/src/index.ts) | package entry point | Core file in the package named by the note: `packages/session-query/tool-session-query`. | `named-package-member` |
| [`packages/session-query/session-query-sqlite/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/session-query/session-query-sqlite/src/index.ts) | package entry point | Core file in the package named by the note: `packages/session-query/session-query-sqlite`. | `named-package-member` |
| [`packages/session-query/tool-session-query/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/session-query/tool-session-query/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/session-query/tool-session-query`. | `named-package-member` |
| [`packages/session-query/session-query-sqlite/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/session-query/session-query-sqlite/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/session-query/session-query-sqlite`. | `named-package-member` |
| [`packages/context/session-reference`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/packages/context/session-reference) | package or module directory | The note names this package or capability. | `named-package` |
| [`packages/session-query/tool-session-query`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/packages/session-query/tool-session-query) | package or module directory | The note names this package or capability. | `named-package` |
| [`packages/session-query/session-query-sqlite`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/packages/session-query/session-query-sqlite) | package or module directory | The note names this package or capability. | `named-package` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `sessionQuery` | `const` | [`packages/host/apiproxy/src/api-proxy.ts:2043`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/src/api-proxy.ts#L2043) | `const sessionQuery = ctx.get('sessionQuery')` |

### Tests and executable evidence

- [`packages/host/apiproxy/tests/api-proxy-fork.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/tests/api-proxy-fork.spec.ts) — A test under the owning area exercises or imports `sessionQuery`.
- [`packages/host/apiproxy/tests/session-export.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/tests/session-export.spec.ts) — A test under the owning area exercises or imports `sessionQuery`.
- [`packages/host/apiproxy/tests/api-proxy-search.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/tests/api-proxy-search.spec.ts) — A test under the owning area exercises or imports `sessionQuery`.
- [`packages/session-query/session-query-sqlite/tests/sqlite.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/session-query/session-query-sqlite/tests/sqlite.spec.ts) — A test under the owning area exercises or imports `sessionQuery`.
- [`packages/session-query/session-query-sqlite/tests/load-path.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/session-query/session-query-sqlite/tests/load-path.e2e.ts) — A test under the owning area exercises or imports `sessionQuery`.
- [`packages/context/session-reference/tests/session-reference.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/context/session-reference/tests/session-reference.spec.ts) — A test under the owning area exercises or imports `session-reference`.
- [`packages/session-query/tool-session-query/tests/sqlite-integration.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/session-query/tool-session-query/tests/sqlite-integration.spec.ts) — A test under the owning area exercises or imports `tool-session-query`. A test under the owning area exercises or imports `session_search`.
- [`packages/session-query/tool-session-query/tests/tool-session-query.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/session-query/tool-session-query/tests/tool-session-query.spec.ts) — A test under the owning area exercises or imports `session_search`. A test under the owning area exercises or imports `session_event_search`.

## How to read the implementation

1. Start with [`packages/bundle/base/cordis.patch.yml`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/bundle/base/cordis.patch.yml) because it has the strongest evidence link to the note.
2. Read public types and entry points before implementation details. Write down the contract in plain language.
3. Follow the named constructs into their callers and tests. Confirm which component can actually enforce the rule.
4. Reconstruct one failure path: cancellation, crash, retry, stale state, partial write, or incompatible provider input—whichever the note is protecting against.
5. Only then map the contract into the current Rust repository. Target paths must come from that repository, never from guesswork in this wiki.

## Rust harness port

**Likely integration point:** Rust provider-adapter boundary.

Preserve the behavior and ownership rule. Do not mechanically translate Cordis, TypeScript declaration merging, Node streams, or package topology into Rust.

Relevant standing rules:

- **`rust/append-log`** — Keep durable facts append-only and derive views from them. Separate the synchronous fact boundary from asynchronous persistence.
- **`rust/context-evidence`** — Compaction may shrink presentation, but it must not erase the evidence ledger, file activity, or durable session facts.
- **`rust/four-tool-surface`** — Preserve the four model tools: read, write, patch, and exec. Reuse internal capabilities without automatically exposing new model-facing tools.
- **`rust/process-discipline`** — Treat this as repository or release policy unless it changes a runtime contract. Do not add runtime machinery for a process-only lesson.
- **`rust/provider-adapter`** — Keep provider wire quirks inside adapters; the loop and durable event vocabulary remain provider-neutral.
- **`rust/quickjs-boundary`** — QuickJS holds orchestration variables; Rust/Tokio owns async execution and exposes only explicitly granted capabilities.
- **`rust/tokio-cancellation`** — Thread one cancellation token through async work, close owned resources before reuse, and test cancellation at every await boundary.
- **`rust/trait-seam`** — Use a small trait at the capability boundary; keep policy and model-facing schemas outside the backend implementation.

### Smallest safe implementation slice

1. Express the semantic contract as Rust types and one owning boundary.
2. Add the smallest adapter or service needed by the existing loop; do not add a new model-facing tool unless the four-tool surface cannot express the job.
3. Record durable evidence before deriving summaries or UI state.
4. Add one happy-path test, one interruption test, and one replay/restart test where state survives a turn.
5. Keep the patch reversible until the source note's strongest failure case is proven in the Rust harness.

## Search handles

- Tags: `class/feature`, `concern/boundary`, `concern/cancellation-timeout`, `concern/performance`, `concern/recovery`, `concern/simplification`, `domain/build-release`, `domain/configuration`, `domain/context`, `domain/extensions`, `domain/llm`, `domain/protocols`, `domain/session-state`, `domain/storage`, `domain/ui-interaction`, `lifecycle/implemented`, `mechanism/projection`
- Aliases: `sessionQuery`, `tool-session-query`, `cordis.patch.yml`, `session_search`, `session_event_search`, `session_trace`, `session_event_trace`, `session_event_read`, `@deepseek-ai/dsh-tool-session-query`, `session-query.cordis.yml`, `ctx.sessionQuery`, `session-query-sqlite`, `session-reference`, `/resume`
- Regex: `(?i)(sessionQuery|tool\-session\-query|cordis\.patch\.yml|session_search|session_event_search|session_trace|session_event_trace|session_event_read)`

```bash
rg -n --pcre2 "(?i)(sessionQuery|tool\\-session\\-query|cordis\\.patch\\.yml|session_search|session_event_search|session_trace|session_event_trace|session_event_read)" source-deepseek-harness
rg -l --fixed-strings "class/feature" 03-agent-context
```

## Connected notes

- **`source-link`** — [0129. Session content search ships opt-in through openAt never](0129-session-content-search-ships-opt-in-through-openat-never.md): The source note links to this decision directly.
- **`source-link`** — [0180. Model-facing session query tools](0180-model-facing-session-query-tools.md): The source note links to this decision directly.
- **`source-link`** — [0229. Even out the shipped tool rosters](0229-even-out-the-shipped-tool-rosters.md): The source note links to this decision directly.
- **`shares-code-with`** — [0252. Web transcript marks context source, recall, and steering](0252-web-transcript-marks-context-source-recall-and-steering.md): Shares source implementation: `packages/context/session-reference/src/index.ts`, `packages/context/session-reference/src/invariant.ts`.
- **`shares-code-with`** — [0197. Web past-session search](0197-web-past-session-search.md): Shares source implementation: `packages/session-query/session-query-sqlite/src/index.ts`, `packages/session-query/session-query-sqlite/src/invariant.ts`.
- **`shares-code-with`** — [0168. Cross-session references](0168-cross-session-references.md): Shares source implementation: `packages/context/session-reference/src/index.ts`, `packages/context/session-reference/src/types.ts`.
- **`shares-code-with`** — [0152. SQLite FTS5 session search](0152-sqlite-fts5-session-search.md): Shares source implementation: `packages/session-query/session-query-sqlite/src/index.ts`, `packages/session-query/session-query-sqlite/src/invariant.ts`.
- **`shares-code-with`** — [0407. CI failover runbook --- hosted pools → in-house pool](0407-ci-failover-runbook-hosted-pools-in-house-pool.md): Shares source implementation: `packages/context/session-reference/src/index.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0243-session-search-tools-are-not-a-shipped-default.md`.
