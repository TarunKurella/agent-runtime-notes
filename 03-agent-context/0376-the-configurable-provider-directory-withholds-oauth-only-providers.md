---
id: "dsh-note-0376"
title: "The configurable-provider directory withholds OAuth-only providers"
status: "implemented"
class: "bug-fix"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/bug-fix/2026-08-13-oauth-only-providers-withheld.md"
implementation_evidence: "medium"
target_anchor: "sandbox capability boundary"
tags:
  - "class/bug-fix"
  - "concern/boundary"
  - "concern/discovery-routing"
  - "concern/ownership"
  - "concern/recovery"
  - "concern/simplification"
  - "domain/build-release"
  - "domain/configuration"
  - "domain/extensions"
  - "domain/llm"
  - "domain/observability"
  - "domain/security"
  - "domain/storage"
  - "domain/testing"
  - "lifecycle/implemented"
  - "mechanism/adapter"
  - "mechanism/registry"
aliases:
  - "declared"
  - "validate"
  - "apiKey"
  - "catalogProviderIds"
  - "catalogProviderTakesApiKey"
  - "resolveProfiles"
  - "directoryEntries"
  - "routeAuth"
  - "LlmError"
  - "yaml"
  - "apiKeyEnv"
  - "openai-codex"
  - "Provider is not configured: openai-codex"
  - "PI_AI_ERROR"
search_regex: "(?i)(declared|validate|apiKey|catalogProviderIds|catalogProviderTakesApiKey|resolveProfiles|directoryEntries|routeAuth)"
---

# 0376. The configurable-provider directory withholds OAuth-only providers — implementation context

## Open this when

The Models page offered openai-codex like any other pi-ai route, with the placeholder every pi-ai provider carries: enter a key, or leave it blank to authenticate from the environment. Configuring it that way and sending a message failed the turn with Provider is not configured: openai-codex, reported as the adapter's catch-all PI_AI_ERROR. The posture the placeholder invited could not work on that route.

## Source decision

The directory offers only what this adapter can authenticate. catalogProviderTakesApiKey(provider) answers whether pi-ai's installed provider for a route declares an api-key method --- the one method the harness can feed, since it resolves a key through its own credential seam and hands it over as the request's apiKey override --- and directoryEntries() skips the catalog routes that fail it. OAuth support is not attempted. It needs a persistent credential store, a login flow, and a surface to run it from; none of those is a release-blocking fix, and shipping the offer without them is what produced the report.

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/bug-fix/2026-08-13-oauth-only-providers-withheld.md](../02-notes/implemented/bug-fix/2026-08-13-oauth-only-providers-withheld.md)
- Pinned source: [.agents/notes/implemented/bug-fix/2026-08-13-oauth-only-providers-withheld.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/bug-fix/2026-08-13-oauth-only-providers-withheld.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`packages/e2b/e2b/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/e2b/e2b/src/index.ts) | package entry point | Defines `apiKey`, a construct named by the note. | `symbol-definition` |
| [`packages/llm/llm/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/src/index.ts) | package entry point | Defines `LlmError`, a construct named by the note. | `symbol-definition` |
| [`packages/core/tools/src/schema.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/schema.ts) | runtime implementation | Defines `validate`, a construct named by the note. | `symbol-definition` |
| [`packages/llm/llm-pi-ai/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm-pi-ai/src/index.ts) | package entry point | Defines `directoryEntries`, a construct named by the note. | `symbol-definition` |
| [`packages/core/tools/src/ts-types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/ts-types.ts) | runtime implementation | Defines `declared`, a construct named by the note. | `symbol-definition` |
| [`packages/llm/llm-pi-ai/src/config.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm-pi-ai/src/config.ts) | runtime implementation | Defines `resolveProfiles`, a construct named by the note. | `symbol-definition` |
| [`packages/llm/llm-pi-ai/src/adapter.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm-pi-ai/src/adapter.ts) | provider/backend adapter | Defines `apiKey`, a construct named by the note. | `symbol-definition` |
| [`packages/llm/llm-pi-ai/src/catalog.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm-pi-ai/src/catalog.ts) | runtime implementation | Defines `catalogProviderTakesApiKey`, a construct named by the note. Defines `catalogProviderIds`, a construct named by the note. | `symbol-definition` |
| [`packages/boot/app-boot/src/profile.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/boot/app-boot/src/profile.ts) | runtime implementation | Defines `declared`, a construct named by the note. | `symbol-definition` |
| [`packages/core/tools/src/json-schema.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/json-schema.ts) | runtime implementation | Defines `declared`, a construct named by the note. | `symbol-definition` |
| [`packages/shell/pwsh-local/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/shell/pwsh-local/src/index.ts) | package entry point | Defines `declared`, a construct named by the note. | `symbol-definition` |
| [`packages/llm/llm-pi-ai/src/provider.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm-pi-ai/src/provider.ts) | provider/backend adapter | Defines `routeAuth`, a construct named by the note. | `symbol-definition` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `declared` | `const` | [`packages/boot/app-boot/src/profile.ts:391`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/boot/app-boot/src/profile.ts#L391) | `const declared = bundleManifest.dsh?.bundle?.patch` |
| `declared` | `const` | [`packages/core/tools/src/json-schema.ts:215`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/json-schema.ts#L215) | `const declared = isJsonSchemaRecord(properties) ? properties : {}` |
| `validate` | `const` | [`packages/core/tools/src/schema.ts:568`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/schema.ts#L568) | `const validate = (args: unknown): string[] => validateJsonSchemaValue(parameters, args, '')` |
| `declared` | `const` | [`packages/core/tools/src/ts-types.ts:168`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/ts-types.ts#L168) | `const declared = typeDocumentFrom(parts)` |
| `apiKey` | `const` | [`packages/e2b/e2b/src/index.ts:94`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/e2b/e2b/src/index.ts#L94) | `const apiKey = config.apiKey ?? process.env.E2B_API_KEY` |
| `apiKey` | `const` | [`packages/llm/llm-deepseek/src/adapter.ts:221`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm-deepseek/src/adapter.ts#L221) | `const apiKey = await this.config.resolveApiKey(connection)` |
| `apiKey` | `const` | [`packages/llm/llm-pi-ai/src/adapter.ts:292`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm-pi-ai/src/adapter.ts#L292) | `const apiKey = await this.config.resolveApiKey(options.provider, profile)` |
| `catalogProviderIds` | `function` | [`packages/llm/llm-pi-ai/src/catalog.ts:140`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm-pi-ai/src/catalog.ts#L140) | `export function catalogProviderIds(): readonly string[] {` |
| `catalogProviderTakesApiKey` | `function` | [`packages/llm/llm-pi-ai/src/catalog.ts:160`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm-pi-ai/src/catalog.ts#L160) | `export function catalogProviderTakesApiKey(provider: string): boolean {` |
| `declared` | `const` | [`packages/llm/llm-pi-ai/src/catalog.ts:341`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm-pi-ai/src/catalog.ts#L341) | `const declared = THINKING_LEVELS.flatMap((level) => {` |
| `resolveProfiles` | `function` | [`packages/llm/llm-pi-ai/src/config.ts:301`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm-pi-ai/src/config.ts#L301) | `export function resolveProfiles(` |
| `apiKey` | `const` | [`packages/llm/llm-pi-ai/src/discovery.ts:241`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm-pi-ai/src/discovery.ts#L241) | `const apiKey = supplied === undefined ? undefined : usableProbeKey(supplied)` |
| `directoryEntries` | `function` | [`packages/llm/llm-pi-ai/src/index.ts:120`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm-pi-ai/src/index.ts#L120) | `function directoryEntries(` |
| `routeAuth` | `function` | [`packages/llm/llm-pi-ai/src/provider.ts:131`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm-pi-ai/src/provider.ts#L131) | `function routeAuth(spec: ProviderSpec, catalog: Provider \| undefined): Provider['auth'] {` |
| `LlmError` | `class` | [`packages/llm/llm/src/index.ts:83`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/src/index.ts#L83) | `export class LlmError extends HarnessError {` |
| `validate` | `function` | [`packages/schedule/schedule/src/invariant.ts:19`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/schedule/schedule/src/invariant.ts#L19) | `function validate(events: readonly SessionEvent[], seedLength: number, fail: InvariantFailure): void {` |

### Tests and executable evidence

- [`scripts/dev-web.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/dev-web.spec.ts) — A test under the owning area exercises or imports `yml`.
- [`scripts/ci-workflow.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/ci-workflow.spec.ts) — A test under the owning area exercises or imports `yml`.
- [`scripts/oxlint-contract.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/oxlint-contract.spec.ts) — A test under the owning area exercises or imports `yml`.
- [`scripts/install-lefthook.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/install-lefthook.spec.ts) — A test under the owning area exercises or imports `yml`.
- [`packages/e2b/e2b/tests/e2b.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/e2b/e2b/tests/e2b.spec.ts) — A test under the owning area exercises or imports `apiKey`.
- [`scripts/cordis-config-files.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/cordis-config-files.spec.ts) — A test under the owning area exercises or imports `yml`.
- [`apps/web/tests/default-model.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/default-model.e2e.ts) — A test under the owning area exercises or imports `models-settings`.
- [`apps/web/tests/models-settings.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/models-settings.e2e.ts) — A test under the owning area exercises or imports `anthropic`. A test under the owning area exercises or imports `models-settings`.
- Source verification intent: Package tests pin both halves of the union: the withheld route is absent from listConfigurableProviders() while anthropic and openai stay, and a stored openai-codex profile still produces a full entry with declared: false. The existing resolution tests are unchanged and still pass, which is what shows the withholding did not narrow what a hand-written profile can serve. The models-settings and onboarding-usable-provider web e2e goldens lost exactly the openai-codex option line, recorded against the real assembled application.

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
- **`rust/process-discipline`** — Treat this as repository or release policy unless it changes a runtime contract. Do not add runtime machinery for a process-only lesson.
- **`rust/provider-adapter`** — Keep provider wire quirks inside adapters; the loop and durable event vocabulary remain provider-neutral.
- **`rust/quickjs-boundary`** — QuickJS holds orchestration variables; Rust/Tokio owns async execution and exposes only explicitly granted capabilities.
- **`rust/trait-seam`** — Use a small trait at the capability boundary; keep policy and model-facing schemas outside the backend implementation.

### Smallest safe implementation slice

1. Express the semantic contract as Rust types and one owning boundary.
2. Add the smallest adapter or service needed by the existing loop; do not add a new model-facing tool unless the four-tool surface cannot express the job.
3. Record durable evidence before deriving summaries or UI state.
4. Add one happy-path test, one interruption test, and one replay/restart test where state survives a turn.
5. Keep the patch reversible until the source note's strongest failure case is proven in the Rust harness.

## Search handles

- Tags: `class/bug-fix`, `concern/boundary`, `concern/discovery-routing`, `concern/ownership`, `concern/recovery`, `concern/simplification`, `domain/build-release`, `domain/configuration`, `domain/extensions`, `domain/llm`, `domain/observability`, `domain/security`, `domain/storage`, `domain/testing`, `lifecycle/implemented`, `mechanism/adapter`, `mechanism/registry`
- Aliases: `declared`, `validate`, `apiKey`, `catalogProviderIds`, `catalogProviderTakesApiKey`, `resolveProfiles`, `directoryEntries`, `routeAuth`, `LlmError`, `yaml`, `apiKeyEnv`, `openai-codex`, `Provider is not configured: openai-codex`, `PI_AI_ERROR`
- Regex: `(?i)(declared|validate|apiKey|catalogProviderIds|catalogProviderTakesApiKey|resolveProfiles|directoryEntries|routeAuth)`

```bash
rg -n --pcre2 "(?i)(declared|validate|apiKey|catalogProviderIds|catalogProviderTakesApiKey|resolveProfiles|directoryEntries|routeAuth)" source-deepseek-harness
rg -l --fixed-strings "class/bug-fix" 03-agent-context
```

## Connected notes

- **`shares-code-with`** — [0077. request-level LLM configuration and the credential seam](0077-request-level-llm-configuration-and-the-credential-seam.md): Shares source implementation: `packages/llm/llm-pi-ai/src/adapter.ts`, `packages/llm/llm-pi-ai/src/catalog.ts`.
- **`shares-code-with`** — [0128. A pi-ai model declares its own input modalities, and undeclared means text](0128-a-pi-ai-model-declares-its-own-input-modalities-and-undeclared-means-tex.md): Shares source implementation: `packages/llm/llm-pi-ai/src/adapter.ts`, `packages/llm/llm-pi-ai/src/catalog.ts`.
- **`shares-code-with`** — [0094. pi-ai routes are declared providers, not catalog lookups](0094-pi-ai-routes-are-declared-providers-not-catalog-lookups.md): Shares source implementation: `packages/llm/llm-pi-ai/src/adapter.ts`, `packages/llm/llm-pi-ai/src/config.ts`.
- **`shares-code-with`** — [0098. Interrogating a draft provider endpoint](0098-interrogating-a-draft-provider-endpoint.md): Shares source implementation: `packages/llm/llm-pi-ai/src/adapter.ts`, `packages/llm/llm-pi-ai/src/catalog.ts`.
- **`shares-code-with`** — [0087. the web configuration plane](0087-the-web-configuration-plane.md): Shares source implementation: `packages/llm/llm-pi-ai/src/adapter.ts`, `packages/llm/llm-pi-ai/src/catalog.ts`.
- **`shares-code-with`** — [0037. Provider-routed LLM adapters and a generic pi-ai backend](0037-provider-routed-llm-adapters-and-a-generic-pi-ai-backend.md): Shares source implementation: `packages/llm/llm-pi-ai/src/config.ts`, `packages/llm/llm-pi-ai/src/index.ts`.
- **`shares-code-with`** — [0097. Declaring a provider from the Models page](0097-declaring-a-provider-from-the-models-page.md): Shares source implementation: `packages/llm/llm-pi-ai/src/adapter.ts`, `packages/llm/llm-pi-ai/src/catalog.ts`.
- **`shares-code-with`** — [0276. Per-Model Reasoning Declarations in llm-pi-ai](0276-per-model-reasoning-declarations-in-llm-pi-ai.md): Shares source implementation: `packages/llm/llm-pi-ai/src/adapter.ts`, `packages/llm/llm-pi-ai/src/catalog.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0376-the-configurable-provider-directory-withholds-oauth-only-providers.md`.
