---
id: "dsh-note-0082"
title: "what the configuration plane exposes, and who may overwrite what"
status: "implemented"
class: "architecture"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/architecture/2026-07-30-config-plane-boundaries.md"
implementation_evidence: "medium"
target_anchor: "sandbox capability boundary"
tags:
  - "class/architecture"
  - "concern/boundary"
  - "concern/compatibility"
  - "concern/concurrency"
  - "concern/discovery-routing"
  - "concern/evidence"
  - "concern/ownership"
  - "concern/recovery"
  - "concern/schema-types"
  - "domain/build-release"
  - "domain/configuration"
  - "domain/extensions"
  - "domain/llm"
  - "domain/protocols"
  - "domain/security"
  - "domain/storage"
  - "domain/web-retrieval"
  - "lifecycle/implemented"
  - "mechanism/generation"
  - "mechanism/policy"
  - "mechanism/registry"
aliases:
  - "trustedHosts"
  - "describe"
  - "apiKey"
  - "headers"
  - "models"
  - "providers"
  - "SettingsDescriptor"
  - "SettingsConflictError"
  - "redactSecrets"
  - "secrets"
  - "revision"
  - "set"
  - "unset"
  - "loading"
search_regex: "(?i)(trustedHosts|describe|apiKey|headers|models|providers|SettingsDescriptor|SettingsConflictError)"
---

# 0082. what the configuration plane exposes, and who may overwrite what — implementation context

## Open this when

The plane worked and was reachable by more callers, and with more authority, than its design claimed. trustedHosts gated only writes, so a declared LAN client could call settings.describe --- every exposed namespace's configuration --- and credentials.describe, which reports whether an arbitrary environment-variable name is configured and where it resolves from. That fence is a DNS-rebinding defense and says so; treating it as an authorization boundary for reads was a category error.

## Source decision

Reading configuration is as privileged as writing it. settings.describe and credentials.describe join the loopback-only set, so the whole configuration plane stays same-origin until real authentication exists. The model catalog (llm.providers, llm.models) deliberately does not: it carries provider ids, display names, and model lists --- no endpoints, no key state --- and a LAN client's model picker needs it. The boundary is asserted over a real HTTP server rather than a hand-assembled request, because the Host header a browser actually sends is what decides it.

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/architecture/2026-07-30-config-plane-boundaries.md](../02-notes/implemented/architecture/2026-07-30-config-plane-boundaries.md)
- Pinned source: [.agents/notes/implemented/architecture/2026-07-30-config-plane-boundaries.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/architecture/2026-07-30-config-plane-boundaries.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`packages/e2b/e2b/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/e2b/e2b/src/index.ts) | package entry point | Defines `apiKey`, a construct named by the note. | `symbol-definition` |
| [`packages/llm/llm/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/src/index.ts) | package entry point | Defines `models`, a construct named by the note. | `symbol-definition` |
| [`vendor/loader/src/config/entry.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/loader/src/config/entry.ts) | runtime implementation | Defines `replace`, a construct named by the note. | `symbol-definition` |
| [`packages/skill/skill/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/skill/skill/src/index.ts) | package entry point | Defines `revision`, a construct named by the note. | `symbol-definition` |
| [`packages/core/tools/src/py-types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/py-types.ts) | runtime implementation | Defines `describe`, a construct named by the note. | `symbol-definition` |
| [`packages/typert/loader/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/typert/loader/src/index.ts) | package entry point | Defines `loading`, a construct named by the note. | `symbol-definition` |
| [`packages/lsp/lsp-stdio/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/lsp/lsp-stdio/src/index.ts) | package entry point | Defines `providers`, a construct named by the note. | `symbol-definition` |
| [`packages/client/connection/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/connection/src/index.ts) | package entry point | Defines `trustedHosts`, a construct named by the note. | `symbol-definition` |
| [`packages/settings/settings/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/settings/settings/src/index.ts) | package entry point | Defines `SettingsConflictError`, a construct named by the note. Defines `SettingsDescriptor`, a construct named by the note. | `symbol-definition` |
| [`packages/settings/settings/src/redact.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/settings/settings/src/redact.ts) | runtime implementation | Defines `redactSecrets`, a construct named by the note. Defines `secrets`, a construct named by the note. | `symbol-definition` |
| [`packages/llm/llm-deepseek/src/adapter.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm-deepseek/src/adapter.ts) | provider/backend adapter | Defines `headers`, a construct named by the note. | `symbol-definition` |
| [`packages/test-support/client-runtime/src/sessions.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/test-support/client-runtime/src/sessions.ts) | runtime implementation | Defines `set`, a construct named by the note. | `symbol-definition` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `trustedHosts` | `const` | [`packages/client/connection/src/index.ts:132`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/connection/src/index.ts#L132) | `const trustedHosts = config?.trustedHosts ?? []` |
| `describe` | `function` | [`packages/core/tools/src/py-types.ts:223`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/py-types.ts#L223) | `function describe(schema: object): string \| undefined {` |
| `apiKey` | `const` | [`packages/e2b/e2b/src/index.ts:94`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/e2b/e2b/src/index.ts#L94) | `const apiKey = config.apiKey ?? process.env.E2B_API_KEY` |
| `headers` | `const` | [`packages/llm/llm-deepseek/src/adapter.ts:283`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm-deepseek/src/adapter.ts#L283) | `const headers = {` |
| `models` | `const` | [`packages/llm/llm/src/index.ts:547`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/src/index.ts#L547) | `const models: LlmDiscoveredModel[] = []` |
| `providers` | `const` | [`packages/lsp/lsp-stdio/src/index.ts:141`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/lsp/lsp-stdio/src/index.ts#L141) | `const providers = await (async () => {` |
| `SettingsDescriptor` | `interface` | [`packages/settings/settings/src/index.ts:65`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/settings/settings/src/index.ts#L65) | `export interface SettingsDescriptor {` |
| `SettingsConflictError` | `class` | [`packages/settings/settings/src/index.ts:164`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/settings/settings/src/index.ts#L164) | `export class SettingsConflictError extends Error {` |
| `redactSecrets` | `function` | [`packages/settings/settings/src/redact.ts:105`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/settings/settings/src/redact.ts#L105) | `export function redactSecrets(schema: z<never>, value: unknown): RedactedValue {` |
| `secrets` | `const` | [`packages/settings/settings/src/redact.ts:106`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/settings/settings/src/redact.ts#L106) | `const secrets: RedactedSecret[] = []` |
| `revision` | `const` | [`packages/skill/skill/src/index.ts:524`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/skill/skill/src/index.ts#L524) | `const revision = this.revision` |
| `set` | `const` | [`packages/test-support/client-runtime/src/sessions.ts:52`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/test-support/client-runtime/src/sessions.ts#L52) | `const set = listeners.get(key) ?? new Set()` |
| `unset` | `const` | [`packages/test-support/client-runtime/src/settings-scope.ts:35`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/test-support/client-runtime/src/settings-scope.ts#L35) | `const unset = vi.fn(() => Promise.resolve())` |
| `loading` | `let` | [`packages/typert/loader/src/index.ts:344`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/typert/loader/src/index.ts#L344) | `let loading = manifests.get(pkgName)` |
| `replace` | `const` | [`vendor/loader/src/config/entry.ts:194`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/loader/src/config/entry.ts#L194) | `const replace = diff.some(key => key === 'name' \|\| key === 'inject' \|\| key === 'group')` |

### Tests and executable evidence

- [`packages/typert/generator/tests/fixtures/type-model/packages/host/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/typert/generator/tests/fixtures/type-model/packages/host/src/index.ts) — Core file in the package named by the note: `packages/typert/generator/tests/fixtures/type-model/packages/host`. A test under the owning area exercises or imports `models`.
- [`packages/typert/generator/tests/fixtures/type-model/packages/host`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/typert/generator/tests/fixtures/type-model/packages/host) — The note names this package or capability.
- [`packages/typert/generator/tests/fixtures/type-model/packages/host/package.json`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/typert/generator/tests/fixtures/type-model/packages/host/package.json) — Core file in the package named by the note: `packages/typert/generator/tests/fixtures/type-model/packages/host`. A test under the owning area exercises or imports `models`.
- [`packages/e2b/e2b/tests/e2b.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/e2b/e2b/tests/e2b.spec.ts) — A test under the owning area exercises or imports `apiKey`.
- [`packages/e2b/e2b/tests/composition.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/e2b/e2b/tests/composition.e2e.ts) — A test under the owning area exercises or imports `apiKey`.
- [`packages/settings/settings/tests/redact.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/settings/settings/tests/redact.spec.ts) — A test under the owning area exercises or imports `redactSecrets`.
- [`packages/settings/settings/tests/settings.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/settings/settings/tests/settings.spec.ts) — A test under the owning area exercises or imports `SettingsConflictError`. A test under the owning area exercises or imports `redactSecrets`.
- [`packages/client/connection/tests/node-half.host.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/connection/tests/node-half.host.spec.ts) — A test under the owning area exercises or imports `trustedHosts`.

## How to read the implementation

1. Start with [`packages/e2b/e2b/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/e2b/e2b/src/index.ts) because it has the strongest evidence link to the note.
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

- Tags: `class/architecture`, `concern/boundary`, `concern/compatibility`, `concern/concurrency`, `concern/discovery-routing`, `concern/evidence`, `concern/ownership`, `concern/recovery`, `concern/schema-types`, `domain/build-release`, `domain/configuration`, `domain/extensions`, `domain/llm`, `domain/protocols`, `domain/security`, `domain/storage`, `domain/web-retrieval`, `lifecycle/implemented`, `mechanism/generation`, `mechanism/policy`, `mechanism/registry`
- Aliases: `trustedHosts`, `describe`, `apiKey`, `headers`, `models`, `providers`, `SettingsDescriptor`, `SettingsConflictError`, `redactSecrets`, `secrets`, `revision`, `set`, `unset`, `loading`
- Regex: `(?i)(trustedHosts|describe|apiKey|headers|models|providers|SettingsDescriptor|SettingsConflictError)`

```bash
rg -n --pcre2 "(?i)(trustedHosts|describe|apiKey|headers|models|providers|SettingsDescriptor|SettingsConflictError)" source-deepseek-harness
rg -l --fixed-strings "class/architecture" 03-agent-context
```

## Connected notes

- **`source-link`** — [0087. the web configuration plane](0087-the-web-configuration-plane.md): The source note links to this decision directly.
- **`shares-code-with`** — [0083. credential boundaries, whole-snapshot requests, and atomic route registration](0083-credential-boundaries-whole-snapshot-requests-and-atomic-route-registrat.md): Shares source implementation: `packages/core/tools/src/py-types.ts`, `packages/llm/llm/src/index.ts`.
- **`shares-code-with`** — [0432. Unified GitHub label taxonomy](0432-unified-github-label-taxonomy.md): Shares source implementation: `packages/core/tools/src/py-types.ts`, `packages/llm/llm/src/index.ts`.
- **`shares-code-with`** — [0126. Repository naming contract and pre-release rename ledger](0126-repository-naming-contract-and-pre-release-rename-ledger.md): Shares source implementation: `packages/e2b/e2b/src/index.ts`, `packages/llm/llm/src/index.ts`.
- **`shares-code-with`** — [0072. Portable consumers over filesystem and subprocess execution worlds](0072-portable-consumers-over-filesystem-and-subprocess-execution-worlds.md): Shares source implementation: `packages/e2b/e2b/src/index.ts`, `packages/lsp/lsp-stdio/src/index.ts`.
- **`shares-code-with`** — [0069. One carrier-level browser-trust boundary for all `/api` routes](0069-one-carrier-level-browser-trust-boundary-for-all-api-routes.md): Shares source implementation: `packages/client/connection/src/index.ts`, `packages/typert/generator/tests/fixtures/type-model/packages/host`.
- **`shares-code-with`** — [0099. WebSocket carrier for browser downlinks](0099-websocket-carrier-for-browser-downlinks.md): Shares source implementation: `packages/client/connection/src/index.ts`, `packages/typert/generator/tests/fixtures/type-model/packages/host/src/index.ts`.
- **`shares-code-with`** — [0516. Migrate simple unary API Proxy calls to business Remote services](0516-migrate-simple-unary-api-proxy-calls-to-business-remote-services.md): Shares source implementation: `packages/llm/llm/src/index.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0082-what-the-configuration-plane-exposes-and-who-may-overwrite-what.md`.
