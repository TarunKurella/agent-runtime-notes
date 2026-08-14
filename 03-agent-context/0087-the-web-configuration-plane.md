---
id: "dsh-note-0087"
title: "the web configuration plane"
status: "implemented"
class: "architecture"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/architecture/2026-07-30-web-config-plane.md"
implementation_evidence: "high"
target_anchor: "sandbox capability boundary"
tags:
  - "class/architecture"
  - "concern/boundary"
  - "concern/cancellation-timeout"
  - "concern/discovery-routing"
  - "concern/evidence"
  - "concern/lifecycle"
  - "concern/ownership"
  - "concern/recovery"
  - "concern/schema-types"
  - "concern/simplification"
  - "domain/build-release"
  - "domain/configuration"
  - "domain/context"
  - "domain/extensions"
  - "domain/filesystem"
  - "domain/llm"
  - "domain/protocols"
  - "domain/security"
  - "domain/shell-terminal"
  - "domain/testing"
  - "domain/ui-interaction"
  - "domain/web-retrieval"
  - "lifecycle/implemented"
  - "mechanism/adapter"
  - "mechanism/policy"
  - "mechanism/registry"
aliases:
  - "user"
  - "mutate"
  - "describe"
  - "openPath"
  - "RpcMethodMap"
  - "action"
  - "contextWindow"
  - "apiKey"
  - "Config"
  - "reasoningEffort"
  - "models"
  - "base"
  - "retryPolicy"
  - "providers"
search_regex: "(?i)(user|mutate|describe|openPath|RpcMethodMap|action|contextWindow|apiKey)"
---

# 0087. the web configuration plane — implementation context

## Open this when

The request-level configuration seam made LLM adapter configuration restart-free, but the only writer was a text editor on settings.yaml: the web client had no wire access to settings, credentials, or provider topology, so "store a key, prompt again" still meant leaving the product. Three gaps blocked a config page rather than one: describe() returned only the merged effective value (a form cannot tell a user override from a composition default, and serializing it would have shipped role('secret') values to every browser), nothing enumerated the providers an adapter could run (a bare-mounted llm-pi-ai was.

## Source decision

Wire domains on the compiled RPC map, rejections as codes, owner events forwarded verbatim. settings.describe/openDocument/update/replace/mutate, credentials.describe/set/unset, llm.providers, and llm.models join RpcMethodMap, so the compiler-locked wiring sites keep schema, handler, and client in lockstep. Seam rejections fold into settings-rejected {ns} / credential-rejected {ref} business errors, while clients subscribe to forwarded settings, credentials, and LLM owner events and converge without polling (forwarded Remote events).

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/architecture/2026-07-30-web-config-plane.md](../02-notes/implemented/architecture/2026-07-30-web-config-plane.md)
- Pinned source: [.agents/notes/implemented/architecture/2026-07-30-web-config-plane.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/architecture/2026-07-30-web-config-plane.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`packages/bundle/base/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/bundle/base/src/index.ts) | package entry point | Core file in the package named by the note: `packages/bundle/base`. | `named-package-member` |
| [`packages/llm/llm-pi-ai/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm-pi-ai/src/index.ts) | package entry point | Core file in the package named by the note: `packages/llm/llm-pi-ai`. | `named-package-member` |
| [`packages/llm/llm-pi-ai/src/stream.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm-pi-ai/src/stream.ts) | runtime implementation | Core file in the package named by the note: `packages/llm/llm-pi-ai`. | `named-package-member` |
| [`packages/bundle/base/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/bundle/base/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/bundle/base`. | `named-package-member` |
| [`packages/llm/llm-pi-ai/src/adapter.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm-pi-ai/src/adapter.ts) | provider/backend adapter | Core file in the package named by the note: `packages/llm/llm-pi-ai`. Defines `models`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/llm/llm-pi-ai/src/catalog.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm-pi-ai/src/catalog.ts) | runtime implementation | Core file in the package named by the note: `packages/llm/llm-pi-ai`. Defines `models`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/llm/llm-deepseek/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm-deepseek/src/index.ts) | package entry point | Core file in the package named by the note: `packages/llm/llm-deepseek`. Defines `Config`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/llm/llm-deepseek/src/types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm-deepseek/src/types.ts) | public types and contract | Core file in the package named by the note: `packages/llm/llm-deepseek`. | `named-package-member` |
| [`packages/llm/llm-pi-ai/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm-pi-ai/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/llm/llm-pi-ai`. | `named-package-member` |
| [`packages/llm/llm-pi-ai/src/discovery.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm-pi-ai/src/discovery.ts) | runtime implementation | Core file in the package named by the note: `packages/llm/llm-pi-ai`. Defines `models`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/client/schema-form/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/schema-form/src/index.ts) | package entry point | Core file in the package named by the note: `packages/client/schema-form`. | `named-package-member` |
| [`packages/llm/llm-deepseek/src/adapter.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm-deepseek/src/adapter.ts) | provider/backend adapter | Core file in the package named by the note: `packages/llm/llm-deepseek`. Defines `contextWindow`, a construct named by the note. | `named-package-member, symbol-definition` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `user` | `const` | [`packages/boot/app-boot/src/index.ts:185`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/boot/app-boot/src/index.ts#L185) | `const user = home === resolve(cwd) ? undefined : readEnvLayer(binName, home, warn)` |
| `mutate` | `const` | [`packages/client/runtime/src/client/contract/store.ts:220`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/runtime/src/client/contract/store.ts#L220) | `const mutate = decl.actions[key] as (draft: T, ...params: unknown[]) => void` |
| `describe` | `function` | [`packages/core/tools/src/py-types.ts:223`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/py-types.ts#L223) | `function describe(schema: object): string \| undefined {` |
| `openPath` | `function` | [`packages/host/apiproxy/src/api-proxy.ts:1899`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/src/api-proxy.ts#L1899) | `function openPath(` |
| `RpcMethodMap` | `interface` | [`packages/host/apiproxy/src/api/rpc-map.ts:24`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/src/api/rpc-map.ts#L24) | `export interface RpcMethodMap {` |
| `action` | `const` | [`packages/jobs/tool-jobs/src/index.ts:149`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/jobs/tool-jobs/src/index.ts#L149) | `const action = '\nDone; job_output.'` |
| `contextWindow` | `const` | [`packages/llm/llm-deepseek/src/adapter.ts:182`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm-deepseek/src/adapter.ts#L182) | `const contextWindow = configured?.contextWindow` |
| `apiKey` | `const` | [`packages/llm/llm-deepseek/src/adapter.ts:221`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm-deepseek/src/adapter.ts#L221) | `const apiKey = await this.config.resolveApiKey(connection)` |
| `Config` | `interface` | [`packages/llm/llm-deepseek/src/index.ts:62`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm-deepseek/src/index.ts#L62) | `export interface Config {` |
| `Config` | `const` | [`packages/llm/llm-deepseek/src/index.ts:91`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm-deepseek/src/index.ts#L91) | `export const Config: z<Config> = z.object({` |
| `reasoningEffort` | `function` | [`packages/llm/llm-deepseek/src/serialize.ts:26`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm-deepseek/src/serialize.ts#L26) | `function reasoningEffort(effort: NonNullable<GenerateOptions['reasoningEffort']>): 'off' \| 'high' \| 'max' {` |
| `models` | `const` | [`packages/llm/llm-pi-ai/src/adapter.ts:202`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm-pi-ai/src/adapter.ts#L202) | `const models: MutableModels = createModels()` |
| `apiKey` | `const` | [`packages/llm/llm-pi-ai/src/adapter.ts:292`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm-pi-ai/src/adapter.ts#L292) | `const apiKey = await this.config.resolveApiKey(options.provider, profile)` |
| `models` | `const` | [`packages/llm/llm-pi-ai/src/catalog.ts:171`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm-pi-ai/src/catalog.ts#L171) | `const models = getBuiltinModels(provider as BuiltinProvider) as Model<Api>[]` |
| `models` | `const` | [`packages/llm/llm-pi-ai/src/catalog.ts:492`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm-pi-ai/src/catalog.ts#L492) | `const models = entries.map((entry) => {` |
| `base` | `const` | [`packages/llm/llm-pi-ai/src/catalog.ts:496`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm-pi-ai/src/catalog.ts#L496) | `const base = defaults.get(entry.id)` |

### Tests and executable evidence

- [`apps/web/tests/models-settings.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/models-settings.e2e.ts) — The source note names this file directly.
- [`packages/llm/llm/tests/service.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/tests/service.spec.ts) — A test under the owning area exercises or imports `retryPolicy`.
- [`packages/llm/llm/tests/retry-policy.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/tests/retry-policy.spec.ts) — A test under the owning area exercises or imports `retryPolicy`.
- [`packages/llm/llm-pi-ai/tests/catalog.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm-pi-ai/tests/catalog.spec.ts) — A test under the owning area exercises or imports `$DSH_HOME`. A test under the owning area exercises or imports `registerConfigurableProviders`.
- [`packages/llm/llm-pi-ai/tests/adapter.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm-pi-ai/tests/adapter.spec.ts) — A test under the owning area exercises or imports `retryPolicy`. A test under the owning area exercises or imports `apiKeyEnv`.
- [`packages/llm/llm-pi-ai/tests/discovery.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm-pi-ai/tests/discovery.spec.ts) — A test under the owning area exercises or imports `apiKeyEnv`. A test under the owning area exercises or imports `apiKey`.
- [`packages/llm/llm-deepseek/tests/adapter.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm-deepseek/tests/adapter.spec.ts) — A test under the owning area exercises or imports `listConfigurableProviders`. A test under the owning area exercises or imports `retryPolicy`.
- [`packages/llm/llm-pi-ai/tests/sdk-options.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm-pi-ai/tests/sdk-options.spec.ts) — A test under the owning area exercises or imports `baseURL`. A test under the owning area exercises or imports `apiKey`.

## How to read the implementation

1. Start with [`packages/bundle/base/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/bundle/base/src/index.ts) because it has the strongest evidence link to the note.
2. Read public types and entry points before implementation details. Write down the contract in plain language.
3. Follow the named constructs into their callers and tests. Confirm which component can actually enforce the rule.
4. Reconstruct one failure path: cancellation, crash, retry, stale state, partial write, or incompatible provider input—whichever the note is protecting against.
5. Only then map the contract into the current Rust repository. Target paths must come from that repository, never from guesswork in this wiki.

## Rust harness port

**Likely integration point:** sandbox capability boundary.

Preserve the behavior and ownership rule. Do not mechanically translate Cordis, TypeScript declaration merging, Node streams, or package topology into Rust.

Relevant standing rules:

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

- Tags: `class/architecture`, `concern/boundary`, `concern/cancellation-timeout`, `concern/discovery-routing`, `concern/evidence`, `concern/lifecycle`, `concern/ownership`, `concern/recovery`, `concern/schema-types`, `concern/simplification`, `domain/build-release`, `domain/configuration`, `domain/context`, `domain/extensions`, `domain/filesystem`, `domain/llm`, `domain/protocols`, `domain/security`, `domain/shell-terminal`, `domain/testing`, `domain/ui-interaction`, `domain/web-retrieval`, `lifecycle/implemented`, `mechanism/adapter`, `mechanism/policy`, `mechanism/registry`
- Aliases: `user`, `mutate`, `describe`, `openPath`, `RpcMethodMap`, `action`, `contextWindow`, `apiKey`, `Config`, `reasoningEffort`, `models`, `base`, `retryPolicy`, `providers`
- Regex: `(?i)(user|mutate|describe|openPath|RpcMethodMap|action|contextWindow|apiKey)`

```bash
rg -n --pcre2 "(?i)(user|mutate|describe|openPath|RpcMethodMap|action|contextWindow|apiKey)" source-deepseek-harness
rg -l --fixed-strings "class/architecture" 03-agent-context
```

## Connected notes

- **`source-link`** — [0077. request-level LLM configuration and the credential seam](0077-request-level-llm-configuration-and-the-credential-seam.md): The source note links to this decision directly.
- **`source-link`** — [0121. Remote event delivery (ctx.remote.$on)](0121-remote-event-delivery-ctx-remote-on.md): The source note links to this decision directly.
- **`source-link`** — [0350. Recoverable provider credential lifecycle](0350-recoverable-provider-credential-lifecycle.md): The source note links to this decision directly.
- **`shares-code-with`** — [0097. Declaring a provider from the Models page](0097-declaring-a-provider-from-the-models-page.md): Shares source implementation: `apps/web/tests/models-settings.e2e.ts`, `packages/llm/llm-deepseek/src/index.ts`.
- **`shares-code-with`** — [0094. pi-ai routes are declared providers, not catalog lookups](0094-pi-ai-routes-are-declared-providers-not-catalog-lookups.md): Shares source implementation: `packages/llm/llm-deepseek/src/index.ts`, `packages/llm/llm-deepseek/src/types.ts`.
- **`shares-code-with`** — [0039. Advisory LLM catalogs and per-session ACP model selection](0039-advisory-llm-catalogs-and-per-session-acp-model-selection.md): Shares source implementation: `packages/llm/llm-deepseek/src/index.ts`, `packages/llm/llm-deepseek/src/types.ts`.
- **`shares-code-with`** — [0098. Interrogating a draft provider endpoint](0098-interrogating-a-draft-provider-endpoint.md): Shares source implementation: `packages/llm/llm-pi-ai/src/adapter.ts`, `packages/llm/llm-pi-ai/src/catalog.ts`.
- **`shares-code-with`** — [0008. Two LLM adapters as a design-verification twin](0008-two-llm-adapters-as-a-design-verification-twin.md): Shares source implementation: `packages/llm/llm-deepseek/src/index.ts`, `packages/llm/llm-deepseek/src/types.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0087-the-web-configuration-plane.md`.
