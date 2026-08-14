---
id: "dsh-note-0500"
title: "Keyless browser e2e lane for the web GUI"
status: "implemented"
class: "testing"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/testing/2026-07-24-web-gui-browser-e2e-lane.md"
implementation_evidence: "high"
target_anchor: "repository tests and engineering policy"
tags:
  - "class/testing"
  - "concern/boundary"
  - "concern/cancellation-timeout"
  - "concern/compatibility"
  - "concern/concurrency"
  - "concern/discovery-routing"
  - "concern/evidence"
  - "concern/human-control"
  - "concern/lifecycle"
  - "concern/ownership"
  - "concern/performance"
  - "concern/recovery"
  - "concern/schema-types"
  - "concern/simplification"
  - "domain/agent-loop"
  - "domain/build-release"
  - "domain/configuration"
  - "domain/context"
  - "domain/extensions"
  - "domain/filesystem"
  - "domain/llm"
  - "domain/observability"
  - "domain/protocols"
  - "domain/security"
  - "domain/session-state"
  - "domain/shell-terminal"
  - "domain/storage"
  - "domain/testing"
  - "domain/ui-interaction"
  - "domain/web-retrieval"
  - "lifecycle/implemented"
  - "mechanism/adapter"
  - "mechanism/event-sourcing"
  - "mechanism/policy"
  - "mechanism/projection"
  - "mechanism/registry"
  - "mechanism/streaming"
aliases:
  - "FixtureApiClient"
  - "json"
  - "refresh"
  - "SessionStore"
  - "seedSession"
  - "persistenceRoot"
  - "dshHome"
  - "list"
  - "InProcessApiClient"
  - "toFetchHandler"
  - "contextWindow"
  - "Context"
  - "JsonlSessionPersistence"
  - "route"
search_regex: "(?i)(FixtureApiClient|json|refresh|SessionStore|seedSession|persistenceRoot|dshHome|list)"
---

# 0500. Keyless browser e2e lane for the web GUI — implementation context

## Open this when

The web GUI ships as a real assembled chain --- chromium page → client plugin bundles → HTTP unary RPC + two SSE streams → toFetchHandler/apiproxy → the host agent loop, tools, and JSONL persistence --- and no test exercised that chain keylessly and deterministically. The GUI testing system covers tier 1 (wire isomorphism in node), tier 2 (object-layer state machines), and tier-3 smokes, but the keyless smoke drives FixtureApiClient --- no host, no wire, no agent loop --- while the full-chain smoke needs DEEPSEEK_API_KEY and a live model, so it is nondeterministic and self-skips in keyless CI.

## Source decision

pnpm run test:web carries a keyless, deterministic browser e2e lane under apps/web/tests/: recorded session-log fixtures replay through @deepseek-ai/dsh-llm-replay against the real in-process web composition, with normalized aria goldens for user-visible states and in-process assertions for durable world state. The supporting product contracts are dsh-llm-replay pacing, consumption checks, and validated indexed override patches; cross-package dsh-llm failures retain validated provider facts through own data properties; and the shipped web composition mounts llm-retry for transient model failures.

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/testing/2026-07-24-web-gui-browser-e2e-lane.md](../02-notes/implemented/testing/2026-07-24-web-gui-browser-e2e-lane.md)
- Pinned source: [.agents/notes/implemented/testing/2026-07-24-web-gui-browser-e2e-lane.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/testing/2026-07-24-web-gui-browser-e2e-lane.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`docs/testing.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/testing.md) | package contract and examples | The source note names this file directly. Contains the exact code literal `dsh-acp-snapshot` named by the note. | `exact-code-occurrence, named-file` |
| [`apps/web/package.json`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/package.json) | composition and configuration | Entry point or contract under the directory named by the note: `apps/web`. The source note names this file directly. | `named-directory-member, named-file` |
| [`packages/llm/llm/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/src/index.ts) | package entry point | Core file in the package named by the note: `packages/llm/llm`. Defines `Context`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/llm/llm/src/types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/src/types.ts) | public types and contract | Core file in the package named by the note: `packages/llm/llm`. | `named-package-member` |
| [`packages/llm/llm/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/llm/llm`. | `named-package-member` |
| [`packages/llm/llm-retry/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm-retry/src/index.ts) | package entry point | Core file in the package named by the note: `packages/llm/llm-retry`. | `named-package-member` |
| [`packages/llm/llm-retry/src/types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm-retry/src/types.ts) | public types and contract | Core file in the package named by the note: `packages/llm/llm-retry`. | `named-package-member` |
| [`packages/llm/llm-deepseek/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm-deepseek/src/index.ts) | package entry point | Core file in the package named by the note: `packages/llm/llm-deepseek`. | `named-package-member` |
| [`packages/llm/llm-deepseek/src/types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm-deepseek/src/types.ts) | public types and contract | Core file in the package named by the note: `packages/llm/llm-deepseek`. | `named-package-member` |
| [`packages/llm/llm-retry/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm-retry/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/llm/llm-retry`. | `named-package-member` |
| [`packages/llm/llm-deepseek/src/adapter.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm-deepseek/src/adapter.ts) | provider/backend adapter | Core file in the package named by the note: `packages/llm/llm-deepseek`. Defines `contextWindow`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/llm/llm-deepseek/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm-deepseek/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/llm/llm-deepseek`. | `named-package-member` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `FixtureApiClient` | `class` | [`packages/client/connection/src/client/fixture.ts:3039`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/connection/src/client/fixture.ts#L3039) | `export class FixtureApiClient extends AbstractApiClient {` |
| `json` | `const` | [`packages/client/modules/src/index.ts:169`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/modules/src/index.ts#L169) | `const json = JSON.stringify(graph).replaceAll('<', '\\u003c')` |
| `refresh` | `const` | [`packages/client/ui-agent-preset/src/client/index.ts:77`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-agent-preset/src/client/index.ts#L77) | `const refresh = (): void => {` |
| `SessionStore` | `class` | [`packages/core/session/src/index.ts:792`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/index.ts#L792) | `export class SessionStore extends Service {` |
| `seedSession` | `const` | [`packages/core/session/src/invariant.ts:207`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/invariant.ts#L207) | `const seedSession = (session: Session): SessionTrace => {` |
| `persistenceRoot` | `const` | [`packages/examples/acp-demo/src/index.ts:115`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/examples/acp-demo/src/index.ts#L115) | `const persistenceRoot = config.persistenceRoot ?? DEFAULT_PERSISTENCE_ROOT` |
| `dshHome` | `const` | [`packages/examples/agent-spine-demo/src/index.ts:218`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/examples/agent-spine-demo/src/index.ts#L218) | `const dshHome = resolveDshHome(config.dshHome ?? nestedDshHome)` |
| `list` | `const` | [`packages/hooks/hook-protocol/src/merge.ts:75`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/hooks/hook-protocol/src/merge.ts#L75) | `const list = reasonsByRank.get(r) ?? []` |
| `InProcessApiClient` | `class` | [`packages/host/apiproxy/src/fetch/client.ts:520`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/src/fetch/client.ts#L520) | `export class InProcessApiClient extends AbstractApiClient {` |
| `toFetchHandler` | `function` | [`packages/host/apiproxy/src/fetch/handler.ts:243`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/src/fetch/handler.ts#L243) | `export function toFetchHandler(api: ApiProxy): { fetch: typeof fetch } {` |
| `contextWindow` | `const` | [`packages/llm/llm-deepseek/src/adapter.ts:182`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm-deepseek/src/adapter.ts#L182) | `const contextWindow = configured?.contextWindow` |
| `Context` | `interface` | [`packages/llm/llm/src/index.ts:47`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/src/index.ts#L47) | `interface Context {` |
| `JsonlSessionPersistence` | `class` | [`packages/session/session-persistence-jsonl/src/index.ts:121`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/session/session-persistence-jsonl/src/index.ts#L121) | `export class JsonlSessionPersistence extends SessionPersistence implements PersistenceBackend<JsonlTornMarker> {` |
| `route` | `const` | [`packages/session/session-title-llm/src/index.ts:245`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/session/session-title-llm/src/index.ts#L245) | `const route = resolveRoute(config, request)` |
| `bundledSkillDir` | `const` | [`packages/skill/skill-filesystem/src/index.ts:171`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/skill/skill-filesystem/src/index.ts#L171) | `const bundledSkillDir = config.bundledSkillDir` |
| `ReplayHandle` | `interface` | [`packages/test-support/llm-replay/src/index.ts:129`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/test-support/llm-replay/src/index.ts#L129) | `export interface ReplayHandle {` |

### Tests and executable evidence

- [`apps/web/tests/scaffold.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/scaffold.ts) — The source note names this file directly. Contains the exact code literal `dsh-acp-snapshot` named by the note.
- [`apps/web/tests/README.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/README.md) — Entry point or contract under the directory named by the note: `apps/web/tests`.
- [`packages/llm/llm/tests/service.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/tests/service.spec.ts) — A test under the owning area exercises or imports `resolveModelInfo`.
- [`packages/llm/llm-retry/tests/retry.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm-retry/tests/retry.spec.ts) — A test under the owning area exercises or imports `whenIdle`.
- [`packages/llm/llm-retry/tests/invariant.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm-retry/tests/invariant.spec.ts) — A test under the owning area exercises or imports `llm-retry`.
- [`packages/llm/llm-deepseek/tests/adapter.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm-deepseek/tests/adapter.spec.ts) — A test under the owning area exercises or imports `resolveModelInfo`. A test under the owning area exercises or imports `DEEPSEEK_BASE_URL`.
- [`packages/llm/llm-retry/tests/persistence.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm-retry/tests/persistence.spec.ts) — A test under the owning area exercises or imports `dsh-session-persistence-jsonl`. A test under the owning area exercises or imports `JsonlSessionPersistence`.
- [`packages/host/apiproxy/tests/fetch-carrier.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/tests/fetch-carrier.spec.ts) — A test under the owning area exercises or imports `toFetchHandler`. A test under the owning area exercises or imports `InProcessApiClient`.
- Source verification intent: pnpm run test:web builds and runs the lane keylessly; test:web:built runs it against existing build artifacts. pnpm run test:web:perf builds and runs the manual performance inventory; test:web:perf:built reuses existing artifacts. DSH_SNAPSHOT=record pnpm exec vitest run --config vitest.web.config.ts apps/web/tests/ records a prompting scenario against the live model, and DSH_SNAPSHOT=refresh pnpm run test:web rewrites aria goldens keylessly. CI explicitly selects replay mode.

## How to read the implementation

1. Start with [`docs/testing.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/testing.md) because it has the strongest evidence link to the note.
2. Read public types and entry points before implementation details. Write down the contract in plain language.
3. Follow the named constructs into their callers and tests. Confirm which component can actually enforce the rule.
4. Reconstruct one failure path: cancellation, crash, retry, stale state, partial write, or incompatible provider input—whichever the note is protecting against.
5. Only then map the contract into the current Rust repository. Target paths must come from that repository, never from guesswork in this wiki.

## Rust harness port

**Likely integration point:** repository tests and engineering policy.

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

- Tags: `class/testing`, `concern/boundary`, `concern/cancellation-timeout`, `concern/compatibility`, `concern/concurrency`, `concern/discovery-routing`, `concern/evidence`, `concern/human-control`, `concern/lifecycle`, `concern/ownership`, `concern/performance`, `concern/recovery`, `concern/schema-types`, `concern/simplification`, `domain/agent-loop`, `domain/build-release`, `domain/configuration`, `domain/context`, `domain/extensions`, `domain/filesystem`, `domain/llm`, `domain/observability`, `domain/protocols`, `domain/security`, `domain/session-state`, `domain/shell-terminal`, `domain/storage`, `domain/testing`, `domain/ui-interaction`, `domain/web-retrieval`, `lifecycle/implemented`, `mechanism/adapter`, `mechanism/event-sourcing`, `mechanism/policy`, `mechanism/projection`, `mechanism/registry`, `mechanism/streaming`
- Aliases: `FixtureApiClient`, `json`, `refresh`, `SessionStore`, `seedSession`, `persistenceRoot`, `dshHome`, `list`, `InProcessApiClient`, `toFetchHandler`, `contextWindow`, `Context`, `JsonlSessionPersistence`, `route`
- Regex: `(?i)(FixtureApiClient|json|refresh|SessionStore|seedSession|persistenceRoot|dshHome|list)`

```bash
rg -n --pcre2 "(?i)(FixtureApiClient|json|refresh|SessionStore|seedSession|persistenceRoot|dshHome|list)" source-deepseek-harness
rg -l --fixed-strings "class/testing" 03-agent-context
```

## Connected notes

- **`source-link`** — [0074. dsh source launch through the tsx ESM hook](0074-dsh-source-launch-through-the-tsx-esm-hook.md): The source note links to this decision directly.
- **`source-link`** — [0351. Reader scroll attribution through the observed-top ledger](0351-reader-scroll-attribution-through-the-observed-top-ledger.md): The source note links to this decision directly.
- **`source-link`** — [0227. The Settings language a fresh browser opens in comes from the browser](0227-the-settings-language-a-fresh-browser-opens-in-comes-from-the-browser.md): The source note links to this decision directly.
- **`source-link`** — [0398. GUI testing system --- the three-tier structure](0398-gui-testing-system-the-three-tier-structure.md): The source note links to this decision directly.
- **`shares-code-with`** — [0037. Provider-routed LLM adapters and a generic pi-ai backend](0037-provider-routed-llm-adapters-and-a-generic-pi-ai-backend.md): Shares source implementation: `packages/llm/llm-retry/src/index.ts`, `packages/llm/llm-retry/src/types.ts`.
- **`shares-code-with`** — [0016. Bounded recovery for transient LLM request failures](0016-bounded-recovery-for-transient-llm-request-failures.md): Shares source implementation: `packages/llm/llm-retry/src/index.ts`, `packages/llm/llm-retry/src/types.ts`.
- **`shares-code-with`** — [0181. Per-provider request retry policies](0181-per-provider-request-retry-policies.md): Shares source implementation: `packages/llm/llm-retry/src/index.ts`, `packages/llm/llm-retry/src/types.ts`.
- **`shares-code-with`** — [0017. Mandatory `User-Agent` attribution for provider requests](0017-mandatory-user-agent-attribution-for-provider-requests.md): Shares source implementation: `packages/llm/llm-deepseek/src/index.ts`, `packages/llm/llm-deepseek/src/types.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0500-keyless-browser-e2e-lane-for-the-web-gui.md`.
