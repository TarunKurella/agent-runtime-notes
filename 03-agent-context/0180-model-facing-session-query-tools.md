---
id: "dsh-note-0180"
title: "Model-facing session query tools"
status: "implemented"
class: "feature"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/feature/2026-07-24-model-facing-session-query-tools.md"
implementation_evidence: "medium"
target_anchor: "sandbox capability boundary"
tags:
  - "class/feature"
  - "concern/boundary"
  - "concern/cancellation-timeout"
  - "concern/discovery-routing"
  - "concern/evidence"
  - "concern/lifecycle"
  - "concern/ownership"
  - "concern/recovery"
  - "concern/schema-types"
  - "concern/trust"
  - "domain/agent-loop"
  - "domain/build-release"
  - "domain/context"
  - "domain/extensions"
  - "domain/filesystem"
  - "domain/jobs-tasks"
  - "domain/llm"
  - "domain/observability"
  - "domain/protocols"
  - "domain/security"
  - "domain/session-state"
  - "domain/storage"
  - "domain/testing"
  - "domain/tools"
  - "domain/ui-interaction"
  - "domain/web-retrieval"
  - "lifecycle/implemented"
  - "mechanism/generation"
  - "mechanism/policy"
  - "mechanism/projection"
  - "mechanism/registry"
aliases:
  - "SessionEventMap"
  - "sessionQuery"
  - "persistedInspectConcurrency"
  - "maxSearchResults"
  - "searchTimeoutMs"
  - "cwd"
  - "before"
  - "after"
  - "spillStore"
  - "ctx.sessionQuery"
  - "@deepseek-ai/dsh-tool-session-query"
  - "session_search"
  - "session_event_search"
  - "session_trace"
search_regex: "(?i)(SessionEventMap|sessionQuery|persistedInspectConcurrency|maxSearchResults|searchTimeoutMs|before|after|spillStore)"
---

# 0180. Model-facing session query tools — implementation context

## Open this when

The unified ctx.sessionQuery service exposes exact reads, filters, relationship traces, and full-text search over live-preferred session logs, but models cannot use that service directly. Giving a model the provider request types would also expose unstable pagination cursors, trusted corpus scope, storage-shaped time values, and result records that are more convenient for programmatic consumers than for reasoning.

## Source decision

@deepseek-ai/dsh-tool-session-query is the model-facing consumer of ctx.sessionQuery. It registers five narrow read-only tools: session_search, session_event_search, session_trace, session_event_trace, and session_event_read. The package imports the interface rather than the SQLite implementation, owns model argument validation and readable text rendering, and contributes one concise prompt section that teaches the prior-history search and search-to-trace/read workflow. The package entrypoint is only the public composition root for configuration, prompt registration, and tool registration.

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/feature/2026-07-24-model-facing-session-query-tools.md](../02-notes/implemented/feature/2026-07-24-model-facing-session-query-tools.md)
- Pinned source: [.agents/notes/implemented/feature/2026-07-24-model-facing-session-query-tools.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/feature/2026-07-24-model-facing-session-query-tools.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`packages/core/agent/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent/src/index.ts) | package entry point | Core file in the package named by the note: `packages/core/agent`. | `named-package-member` |
| [`packages/core/agent/src/types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent/src/types.ts) | public types and contract | Core file in the package named by the note: `packages/core/agent`. Defines `SessionEventMap`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/core/agent/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/core/agent`. | `named-package-member` |
| [`packages/session-query/tool-session-query/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/session-query/tool-session-query/src/index.ts) | package entry point | Core file in the package named by the note: `packages/session-query/tool-session-query`. Defines `maxSearchResults`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/session-query/tool-session-query/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/session-query/tool-session-query/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/session-query/tool-session-query`. | `named-package-member` |
| [`packages/session-query/tool-session-query/src/operations.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/session-query/tool-session-query/src/operations.ts) | runtime implementation | Core file in the package named by the note: `packages/session-query/tool-session-query`. Defines `cwd`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/session-query/tool-session-query/src/presentation.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/session-query/tool-session-query/src/presentation.ts) | runtime implementation | Core file in the package named by the note: `packages/session-query/tool-session-query`. Defines `before`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/session-query/tool-session-query/src/workspace-access.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/session-query/tool-session-query/src/workspace-access.ts) | runtime implementation | Core file in the package named by the note: `packages/session-query/tool-session-query`. Defines `cwd`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/core/agent`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent) | package or module directory | The note names this package or capability. | `named-package` |
| [`packages/session-query/tool-session-query`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/packages/session-query/tool-session-query) | package or module directory | The note names this package or capability. | `named-package` |
| [`packages/host/apiproxy/src/api-proxy.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/src/api-proxy.ts) | runtime implementation | Defines `sessionQuery`, a construct named by the note. | `symbol-definition` |
| [`packages/spill/spill-policy/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/spill/spill-policy/src/index.ts) | package entry point | Defines `spillStore`, a construct named by the note. | `symbol-definition` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `SessionEventMap` | `interface` | [`packages/core/agent/src/types.ts:13`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent/src/types.ts#L13) | `interface SessionEventMap {` |
| `sessionQuery` | `const` | [`packages/host/apiproxy/src/api-proxy.ts:2043`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/src/api-proxy.ts#L2043) | `const sessionQuery = ctx.get('sessionQuery')` |
| `persistedInspectConcurrency` | `const` | [`packages/session-query/session-query/src/index.ts:96`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/session-query/session-query/src/index.ts#L96) | `const persistedInspectConcurrency = config.persistedInspectConcurrency` |
| `maxSearchResults` | `const` | [`packages/session-query/tool-session-query/src/index.ts:126`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/session-query/tool-session-query/src/index.ts#L126) | `const maxSearchResults = config.maxSearchResults ?? DEFAULT_MAX_SEARCH_RESULTS` |
| `searchTimeoutMs` | `const` | [`packages/session-query/tool-session-query/src/index.ts:127`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/session-query/tool-session-query/src/index.ts#L127) | `const searchTimeoutMs = config.searchTimeoutMs ?? DEFAULT_SEARCH_TIMEOUT_MS` |
| `cwd` | `const` | [`packages/session-query/tool-session-query/src/operations.ts:61`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/session-query/tool-session-query/src/operations.ts#L61) | `const cwd = caller.header.cwd` |
| `before` | `const` | [`packages/session-query/tool-session-query/src/presentation.ts:170`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/session-query/tool-session-query/src/presentation.ts#L170) | `const before = window.events.filter(event => event.seq < window.target.seq)` |
| `after` | `const` | [`packages/session-query/tool-session-query/src/presentation.ts:171`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/session-query/tool-session-query/src/presentation.ts#L171) | `const after = window.events.filter(event => event.seq > window.target.seq)` |
| `cwd` | `const` | [`packages/session-query/tool-session-query/src/workspace-access.ts:80`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/session-query/tool-session-query/src/workspace-access.ts#L80) | `const cwd = caller.header.cwd` |
| `cwd` | `const` | [`packages/session-query/tool-session-query/src/workspace-access.ts:118`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/session-query/tool-session-query/src/workspace-access.ts#L118) | `const cwd = caller.header.cwd` |
| `spillStore` | `const` | [`packages/spill/spill-policy/src/index.ts:142`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/spill/spill-policy/src/index.ts#L142) | `const spillStore = ctx.get('spillStore')` |

### Tests and executable evidence

- [`packages/host/apiproxy/tests/api-proxy-fork.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/tests/api-proxy-fork.spec.ts) — A test under the owning area exercises or imports `sessionQuery`.
- [`packages/host/apiproxy/tests/session-export.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/tests/session-export.spec.ts) — A test under the owning area exercises or imports `sessionQuery`.
- [`packages/host/apiproxy/tests/api-proxy-search.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/tests/api-proxy-search.spec.ts) — A test under the owning area exercises or imports `sessionQuery`.
- [`packages/spill/spill-policy/tests/spill-policy.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/spill/spill-policy/tests/spill-policy.spec.ts) — A test under the owning area exercises or imports `spillStore`.
- [`packages/session-query/session-query/tests/session-query.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/session-query/session-query/tests/session-query.spec.ts) — A test under the owning area exercises or imports `persistedInspectConcurrency`.
- [`packages/session-query/tool-session-query/tests/tool-session-query.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/session-query/tool-session-query/tests/tool-session-query.spec.ts) — A test under the owning area exercises or imports `sessionQuery`. A test under the owning area exercises or imports `session_search`.
- [`packages/session-query/tool-session-query/tests/sqlite-integration.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/session-query/tool-session-query/tests/sqlite-integration.spec.ts) — A test under the owning area exercises or imports `session_search`. A test under the owning area exercises or imports `session_event_search`.
- Source verification intent: Package tests pin argument validation, filter translation, timestamp normalization, exact-workspace authorization, parent-filter preauthorization and oracle resistance, changed-observation rejection, service-diagnostic redaction for ordinary and adversarial unknown values, best-effort cyclic-cause logging, logger-failure containment, missing-identity behavior, hidden-boundary pruning, current-step exclusion, internal provider paging, exclusive search and parallel exact-read classification, count caps, exact-signal forwarding, abort-reason preservation, persistence cleanup quiescence, one-scan bounded batch.

## How to read the implementation

1. Start with [`packages/core/agent/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent/src/index.ts) because it has the strongest evidence link to the note.
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
- **`rust/tokio-cancellation`** — Thread one cancellation token through async work, close owned resources before reuse, and test cancellation at every await boundary.
- **`rust/trait-seam`** — Use a small trait at the capability boundary; keep policy and model-facing schemas outside the backend implementation.

### Smallest safe implementation slice

1. Express the semantic contract as Rust types and one owning boundary.
2. Add the smallest adapter or service needed by the existing loop; do not add a new model-facing tool unless the four-tool surface cannot express the job.
3. Record durable evidence before deriving summaries or UI state.
4. Add one happy-path test, one interruption test, and one replay/restart test where state survives a turn.
5. Keep the patch reversible until the source note's strongest failure case is proven in the Rust harness.

## Search handles

- Tags: `class/feature`, `concern/boundary`, `concern/cancellation-timeout`, `concern/discovery-routing`, `concern/evidence`, `concern/lifecycle`, `concern/ownership`, `concern/recovery`, `concern/schema-types`, `concern/trust`, `domain/agent-loop`, `domain/build-release`, `domain/context`, `domain/extensions`, `domain/filesystem`, `domain/jobs-tasks`, `domain/llm`, `domain/observability`, `domain/protocols`, `domain/security`, `domain/session-state`, `domain/storage`, `domain/testing`, `domain/tools`, `domain/ui-interaction`, `domain/web-retrieval`, `lifecycle/implemented`, `mechanism/generation`, `mechanism/policy`, `mechanism/projection`, `mechanism/registry`
- Aliases: `SessionEventMap`, `sessionQuery`, `persistedInspectConcurrency`, `maxSearchResults`, `searchTimeoutMs`, `cwd`, `before`, `after`, `spillStore`, `ctx.sessionQuery`, `@deepseek-ai/dsh-tool-session-query`, `session_search`, `session_event_search`, `session_trace`
- Regex: `(?i)(SessionEventMap|sessionQuery|persistedInspectConcurrency|maxSearchResults|searchTimeoutMs|before|after|spillStore)`

```bash
rg -n --pcre2 "(?i)(SessionEventMap|sessionQuery|persistedInspectConcurrency|maxSearchResults|searchTimeoutMs|before|after|spillStore)" source-deepseek-harness
rg -l --fixed-strings "class/feature" 03-agent-context
```

## Connected notes

- **`source-link`** — [0243. Session search tools are not a shipped default](0243-session-search-tools-are-not-a-shipped-default.md): The source note links to this decision directly.
- **`shares-code-with`** — [0038. Initiating Agent scope over AsyncLocalStorage](0038-initiating-agent-scope-over-asynclocalstorage.md): Shares source implementation: `packages/core/agent`, `packages/core/agent/src/index.ts`.
- **`shares-code-with`** — [0363. Bounded background job admission](0363-bounded-background-job-admission.md): Shares source implementation: `packages/core/agent`, `packages/core/agent/src/index.ts`.
- **`shares-code-with`** — [0146. The subprocess sandbox --- confinement seam, native runners, escalation, and per-session modes](0146-the-subprocess-sandbox-confinement-seam-native-runners-escalation-and-pe.md): Shares source implementation: `packages/core/agent/src/index.ts`, `packages/core/agent/src/invariant.ts`.
- **`shares-code-with`** — [0150. Expose agent session identity and JSONL location to tools and hooks](0150-expose-agent-session-identity-and-jsonl-location-to-tools-and-hooks.md): Shares source implementation: `packages/core/agent`, `packages/core/agent/src/index.ts`.
- **`shares-code-with`** — [0035. Agent-scope runtime design and correctness](0035-agent-scope-runtime-design-and-correctness.md): Shares source implementation: `packages/core/agent/src/index.ts`, `packages/core/agent/src/invariant.ts`.
- **`shares-code-with`** — [0015. The background job runtime (`ctx.jobs`) and generic task control tools](0015-the-background-job-runtime-ctx-jobs-and-generic-task-control-tools.md): Shares source implementation: `packages/core/agent/src/index.ts`, `packages/core/agent/src/invariant.ts`.
- **`shares-code-with`** — [0042. Explicit turn cancellation capability](0042-explicit-turn-cancellation-capability.md): Shares source implementation: `packages/core/agent`, `packages/core/agent/src/index.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0180-model-facing-session-query-tools.md`.
