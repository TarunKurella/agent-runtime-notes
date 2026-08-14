---
id: "dsh-note-0083"
title: "credential boundaries, whole-snapshot requests, and atomic route registration"
status: "implemented"
class: "architecture"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/architecture/2026-07-30-credential-boundaries-and-atomic-registration.md"
implementation_evidence: "medium"
target_anchor: "sandbox capability boundary"
tags:
  - "class/architecture"
  - "concern/boundary"
  - "concern/cancellation-timeout"
  - "concern/concurrency"
  - "concern/discovery-routing"
  - "concern/evidence"
  - "concern/ownership"
  - "concern/performance"
  - "concern/trust"
  - "domain/configuration"
  - "domain/extensions"
  - "domain/filesystem"
  - "domain/llm"
  - "domain/security"
  - "domain/shell-terminal"
  - "domain/storage"
  - "domain/ui-interaction"
  - "lifecycle/implemented"
  - "mechanism/adapter"
  - "mechanism/generation"
  - "mechanism/policy"
  - "mechanism/registry"
aliases:
  - "describe"
  - "DeepSeekConnectionOptions"
  - "resolveApiKey"
  - "LlmAdapter"
  - "installSettingsSection"
  - "env"
  - "set"
  - "unset"
  - "apiKeyEnv"
  - "replace"
  - "credentials-local"
  - "dsh-atomic-write"
  - "$DSH_HOME/.env"
  - "process.env"
search_regex: "(?i)(describe|DeepSeekConnectionOptions|resolveApiKey|LlmAdapter|installSettingsSection|unset|apiKeyEnv|replace)"
---

# 0083. credential boundaries, whole-snapshot requests, and atomic route registration — implementation context

## Open this when

The credential path leaked across boundaries it had drawn. The shipped surfaces hoisted $DSH_HOME/.env into process.env before cordis booted, so on the next run credentials-local classified every key it had stored itself as a read-only ambient launch override: describe() reported source: 'env' with writable: false, set/unset rejected as shadowed, and a key stored from the web page or TUI became unrotatable and undeletable while the adapter kept using the value captured at launch.

## Source decision

The credential document belongs to the credential provider alone. No surface loads it into process.env. It was $DSH_HOME/.env here; the credentials document split later moved it to $DSH_HOME/.credentials.yaml, so today it is the old path that is loaded --- as the user's ordinary environment layer, holding no provider-managed secret. The genuine launch environment and the invoking directory's .env (loaded by the bin) stay the read-only ambient layer, so a composition without the provider resolves keys exactly as before, while a stored key stays file-sourced and writable across restarts --- proven by a real.

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/architecture/2026-07-30-credential-boundaries-and-atomic-registration.md](../02-notes/implemented/architecture/2026-07-30-credential-boundaries-and-atomic-registration.md)
- Pinned source: [.agents/notes/implemented/architecture/2026-07-30-credential-boundaries-and-atomic-registration.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/architecture/2026-07-30-credential-boundaries-and-atomic-registration.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`packages/util/atomic-write/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/util/atomic-write/src/index.ts) | package entry point | Core file in the package named by the note: `packages/util/atomic-write`. | `named-package-member` |
| [`packages/util/atomic-write/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/util/atomic-write/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/util/atomic-write`. | `named-package-member` |
| [`packages/credentials/credentials-local/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/credentials/credentials-local/src/index.ts) | package entry point | Core file in the package named by the note: `packages/credentials/credentials-local`. | `named-package-member` |
| [`packages/credentials/credentials-local/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/credentials/credentials-local/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/credentials/credentials-local`. | `named-package-member` |
| [`packages/util/atomic-write`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/packages/util/atomic-write) | package or module directory | The note names this package or capability. | `named-package` |
| [`packages/credentials/credentials-local`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/packages/credentials/credentials-local) | package or module directory | The note names this package or capability. | `named-package` |
| [`packages/llm/llm/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/src/index.ts) | package entry point | Defines `LlmAdapter`, a construct named by the note. | `symbol-definition` |
| [`vendor/loader/src/config/entry.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/loader/src/config/entry.ts) | runtime implementation | Defines `replace`, a construct named by the note. | `symbol-definition` |
| [`packages/core/tools/src/py-types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/py-types.ts) | runtime implementation | Defines `describe`, a construct named by the note. | `symbol-definition` |
| [`packages/llm/llm-pi-ai/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm-pi-ai/src/index.ts) | package entry point | Defines `resolveApiKey`, a construct named by the note. | `symbol-definition` |
| [`packages/settings/settings/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/settings/settings/src/index.ts) | package entry point | Defines `installSettingsSection`, a construct named by the note. | `symbol-definition` |
| [`packages/llm/llm-deepseek/src/adapter.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm-deepseek/src/adapter.ts) | provider/backend adapter | Defines `DeepSeekConnectionOptions`, a construct named by the note. | `symbol-definition` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `describe` | `function` | [`packages/core/tools/src/py-types.ts:223`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/py-types.ts#L223) | `function describe(schema: object): string \| undefined {` |
| `DeepSeekConnectionOptions` | `interface` | [`packages/llm/llm-deepseek/src/adapter.ts:49`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm-deepseek/src/adapter.ts#L49) | `export interface DeepSeekConnectionOptions {` |
| `resolveApiKey` | `const` | [`packages/llm/llm-pi-ai/src/index.ts:175`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm-pi-ai/src/index.ts#L175) | `const resolveApiKey = async (` |
| `LlmAdapter` | `class` | [`packages/llm/llm/src/index.ts:180`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/src/index.ts#L180) | `export abstract class LlmAdapter {` |
| `installSettingsSection` | `function` | [`packages/settings/settings/src/index.ts:863`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/settings/settings/src/index.ts#L863) | `export function installSettingsSection<T>(` |
| `env` | `const` | [`packages/subprocess/subprocess/src/index.ts:61`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/subprocess/subprocess/src/index.ts#L61) | `const env: Record<string, string> = {}` |
| `set` | `const` | [`packages/test-support/client-runtime/src/sessions.ts:52`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/test-support/client-runtime/src/sessions.ts#L52) | `const set = listeners.get(key) ?? new Set()` |
| `unset` | `const` | [`packages/test-support/client-runtime/src/settings-scope.ts:35`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/test-support/client-runtime/src/settings-scope.ts#L35) | `const unset = vi.fn(() => Promise.resolve())` |
| `apiKeyEnv` | `const` | [`packages/web/web-search-deepseek/src/index.ts:96`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/web/web-search-deepseek/src/index.ts#L96) | `const apiKeyEnv = credentialRef(config.apiKeyEnv ?? DEFAULT_API_KEY_ENV)` |
| `replace` | `const` | [`vendor/loader/src/config/entry.ts:194`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/loader/src/config/entry.ts#L194) | `const replace = diff.some(key => key === 'name' \|\| key === 'inject' \|\| key === 'group')` |

### Tests and executable evidence

- [`packages/llm/llm/tests/service.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/tests/service.spec.ts) — A test under the owning area exercises or imports `LlmAdapter`.
- [`packages/llm/llm/tests/topology.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/tests/topology.spec.ts) — A test under the owning area exercises or imports `LlmAdapter`.
- [`packages/llm/llm/tests/invariant.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/tests/invariant.spec.ts) — A test under the owning area exercises or imports `LlmAdapter`.
- [`packages/llm/llm-pi-ai/tests/catalog.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm-pi-ai/tests/catalog.spec.ts) — A test under the owning area exercises or imports `resolveApiKey`.
- [`packages/llm/llm-pi-ai/tests/adapter.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm-pi-ai/tests/adapter.spec.ts) — A test under the owning area exercises or imports `resolveApiKey`.
- [`packages/llm/llm-pi-ai/tests/sdk-options.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm-pi-ai/tests/sdk-options.spec.ts) — A test under the owning area exercises or imports `resolveApiKey`.
- [`packages/settings/settings/tests/settings.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/settings/settings/tests/settings.spec.ts) — A test under the owning area exercises or imports `installSettingsSection`.
- [`packages/util/atomic-write/tests/invariant.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/util/atomic-write/tests/invariant.spec.ts) — A test under the owning area exercises or imports `dsh-atomic-write`.

## How to read the implementation

1. Start with [`packages/util/atomic-write/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/util/atomic-write/src/index.ts) because it has the strongest evidence link to the note.
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

- Tags: `class/architecture`, `concern/boundary`, `concern/cancellation-timeout`, `concern/concurrency`, `concern/discovery-routing`, `concern/evidence`, `concern/ownership`, `concern/performance`, `concern/trust`, `domain/configuration`, `domain/extensions`, `domain/filesystem`, `domain/llm`, `domain/security`, `domain/shell-terminal`, `domain/storage`, `domain/ui-interaction`, `lifecycle/implemented`, `mechanism/adapter`, `mechanism/generation`, `mechanism/policy`, `mechanism/registry`
- Aliases: `describe`, `DeepSeekConnectionOptions`, `resolveApiKey`, `LlmAdapter`, `installSettingsSection`, `env`, `set`, `unset`, `apiKeyEnv`, `replace`, `credentials-local`, `dsh-atomic-write`, `$DSH_HOME/.env`, `process.env`
- Regex: `(?i)(describe|DeepSeekConnectionOptions|resolveApiKey|LlmAdapter|installSettingsSection|unset|apiKeyEnv|replace)`

```bash
rg -n --pcre2 "(?i)(describe|DeepSeekConnectionOptions|resolveApiKey|LlmAdapter|installSettingsSection|unset|apiKeyEnv|replace)" source-deepseek-harness
rg -l --fixed-strings "class/architecture" 03-agent-context
```

## Connected notes

- **`source-link`** — [0077. request-level LLM configuration and the credential seam](0077-request-level-llm-configuration-and-the-credential-seam.md): The source note links to this decision directly.
- **`source-link`** — [0086. settings write-path integrity and observer lifecycle](0086-settings-write-path-integrity-and-observer-lifecycle.md): The source note links to this decision directly.
- **`source-link`** — [0096. Splitting the credential store from the user environment layer](0096-splitting-the-credential-store-from-the-user-environment-layer.md): The source note links to this decision directly.
- **`shares-code-with`** — [0082. what the configuration plane exposes, and who may overwrite what](0082-what-the-configuration-plane-exposes-and-who-may-overwrite-what.md): Shares source implementation: `packages/core/tools/src/py-types.ts`, `packages/llm/llm/src/index.ts`.
- **`shares-code-with`** — [0037. Provider-routed LLM adapters and a generic pi-ai backend](0037-provider-routed-llm-adapters-and-a-generic-pi-ai-backend.md): Shares source implementation: `packages/llm/llm-pi-ai/src/index.ts`, `packages/llm/llm/src/index.ts`.
- **`shares-code-with`** — [0131. Code Mode --- the model writes TypeScript against the tool registry](0131-code-mode-the-model-writes-typescript-against-the-tool-registry.md): Shares source implementation: `packages/llm/llm/src/index.ts`.
- **`shares-code-with`** — [0078. Terminal LLM stream failures](0078-terminal-llm-stream-failures.md): Shares source implementation: `packages/core/tools/src/py-types.ts`, `packages/llm/llm/src/index.ts`.
- **`shares-code-with`** — [0126. Repository naming contract and pre-release rename ledger](0126-repository-naming-contract-and-pre-release-rename-ledger.md): Shares source implementation: `packages/llm/llm/src/index.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0083-credential-boundaries-whole-snapshot-requests-and-atomic-route-registrat.md`.
