---
id: "dsh-note-0346"
title: "Validate API key format before it reaches an HTTP header"
status: "implemented"
class: "bug-fix"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/bug-fix/2026-08-06-api-key-format-validation.md"
implementation_evidence: "high"
target_anchor: "sandbox capability boundary"
tags:
  - "class/bug-fix"
  - "concern/boundary"
  - "concern/discovery-routing"
  - "concern/evidence"
  - "concern/human-control"
  - "concern/lifecycle"
  - "concern/ownership"
  - "concern/schema-types"
  - "domain/build-release"
  - "domain/configuration"
  - "domain/extensions"
  - "domain/llm"
  - "domain/security"
  - "domain/shell-terminal"
  - "domain/storage"
  - "domain/testing"
  - "domain/ui-interaction"
  - "domain/web-retrieval"
  - "lifecycle/implemented"
  - "mechanism/adapter"
  - "mechanism/policy"
  - "mechanism/projection"
aliases:
  - "empty"
  - "CustomProviderCard"
  - "modelFailure"
  - "validateDeepSeekModels"
  - "ProviderEditor"
  - "stringAt"
  - "apiKeyFailure"
  - "length"
  - "catalogModel"
  - "resolveApiKey"
  - "discoverModels"
  - "routeAuth"
  - "normalizeApiKey"
  - "INVALID_CREDENTIAL_CODE"
search_regex: "(?i)(empty|CustomProviderCard|modelFailure|validateDeepSeekModels|ProviderEditor|stringAt|apiKeyFailure|length)"
---

# 0346. Validate API key format before it reaches an HTTP header — implementation context

## Open this when

An API key holding characters no HTTP header value can carry was accepted by every configuration surface and failed only when a request was built, far from the field that caused it. Pasting a key containing an emoji, CJK text, or a full-width punctuation mark into the web Models page reported a successful save. The first turn then failed with Cannot convert argument to a ByteString because the character at index 7 has a value of 55357 which is greater than 255 --- the index and code point are UTF-16 internals with no action attached, and they disclose the code point of one character of the key.

## Source decision

One rule defines a legal key: after trimming, non-empty, and every character within [\x21-\x7E] --- printable ASCII, space excluded. This single predicate covers every reported input: empty, leading and trailing whitespace, interior whitespace, C0 control characters, emoji, CJK text, and full-width punctuation. It is also exactly the constraint that produced the ByteString failure, so the failures share one definition rather than two coincidentally related fixes. A second, narrower rule catches a pasted environment line: input matching ^[A-Z][A-Z0-9_]=[^=] or wrapped in matching quotes is refused.

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/bug-fix/2026-08-06-api-key-format-validation.md](../02-notes/implemented/bug-fix/2026-08-06-api-key-format-validation.md)
- Pinned source: [.agents/notes/implemented/bug-fix/2026-08-06-api-key-format-validation.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/bug-fix/2026-08-06-api-key-format-validation.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`packages/llm/llm/src/attribution.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/src/attribution.ts) | runtime implementation | The source note names this file directly. | `named-file` |
| [`packages/llm/llm-pi-ai/src/provider.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm-pi-ai/src/provider.ts) | provider/backend adapter | The source note names this file directly. Core file in the package named by the note: `packages/llm/llm-pi-ai`. | `named-file, named-package-member, symbol-definition` |
| [`packages/llm/llm-pi-ai/src/discovery.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm-pi-ai/src/discovery.ts) | runtime implementation | The source note names this file directly. Core file in the package named by the note: `packages/llm/llm-pi-ai`. | `named-file, named-package-member, symbol-definition` |
| [`packages/llm/llm-deepseek/src/adapter.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm-deepseek/src/adapter.ts) | provider/backend adapter | The source note names this file directly. | `named-file` |
| [`packages/llm/llm/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/src/index.ts) | package entry point | Core file in the package named by the note: `packages/llm/llm`. Defines `LlmError`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/llm/llm/src/types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/src/types.ts) | public types and contract | Core file in the package named by the note: `packages/llm/llm`. | `named-package-member` |
| [`packages/llm/llm/src/error.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/src/error.ts) | runtime implementation | Core file in the package named by the note: `packages/llm/llm`. Defines `INVALID_CREDENTIAL_CODE`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/llm/llm/src/api-key.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/src/api-key.ts) | runtime implementation | Core file in the package named by the note: `packages/llm/llm`. Defines `normalizeApiKey`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/llm/llm/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/llm/llm`. | `named-package-member` |
| [`packages/llm/llm-pi-ai/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm-pi-ai/src/index.ts) | package entry point | Core file in the package named by the note: `packages/llm/llm-pi-ai`. Defines `resolveApiKey`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/llm/llm/src/retry-policy.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/src/retry-policy.ts) | runtime implementation | Core file in the package named by the note: `packages/llm/llm`. Defines `DEFAULT_RETRYABLE_CODES`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/llm/llm-deepseek/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm-deepseek/src/index.ts) | package entry point | Core file in the package named by the note: `packages/llm/llm-deepseek`. Defines `catalogModel`, a construct named by the note. | `named-package-member, symbol-definition` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `empty` | `const` | [`packages/client/ui-primitives/src/WebBlock.tsx:160`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-primitives/src/WebBlock.tsx#L160) | `const empty = (answer === undefined \|\| answer === '') && sources.length === 0` |
| `CustomProviderCard` | `function` | [`packages/client/ui-settings-models/src/client/CustomProviderCard.tsx:76`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-settings-models/src/client/CustomProviderCard.tsx#L76) | `export function CustomProviderCard(props: CustomProviderCardProps): ReactNode {` |
| `modelFailure` | `const` | [`packages/client/ui-settings-models/src/client/CustomProviderCard.tsx:104`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-settings-models/src/client/CustomProviderCard.tsx#L104) | `const modelFailure = validateDeepSeekModels(models)` |
| `validateDeepSeekModels` | `function` | [`packages/client/ui-settings-models/src/client/DeepSeekModelsEditor.tsx:94`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-settings-models/src/client/DeepSeekModelsEditor.tsx#L94) | `export function validateDeepSeekModels(value: unknown): DeepSeekModelsValidationFailure \| undefined {` |
| `ProviderEditor` | `function` | [`packages/client/ui-settings-models/src/client/ProviderEditor.tsx:145`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-settings-models/src/client/ProviderEditor.tsx#L145) | `export function ProviderEditor(props: ProviderEditorProps): ReactNode {` |
| `stringAt` | `const` | [`packages/client/ui-settings-models/src/client/ProviderEditor.tsx:191`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-settings-models/src/client/ProviderEditor.tsx#L191) | `const stringAt = (source: unknown, key: string): string \| undefined => {` |
| `modelFailure` | `const` | [`packages/client/ui-settings-models/src/client/ProviderEditor.tsx:206`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-settings-models/src/client/ProviderEditor.tsx#L206) | `const modelFailure = validateDeepSeekModels(getPath(draft, ['models']))` |
| `apiKeyFailure` | `function` | [`packages/client/ui-settings-models/src/client/apiKey.ts:51`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-settings-models/src/client/apiKey.ts#L51) | `export function apiKeyFailure(draft: string): ApiKeyFailureKey \| undefined {` |
| `length` | `const` | [`packages/fs/fs-local/src/fsio.ts:717`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/fs-local/src/fsio.ts#L717) | `const length = Math.min(buffer.length - total, DIFF_BASIS_READ_CHUNK_BYTES)` |
| `catalogModel` | `const` | [`packages/llm/llm-deepseek/src/index.ts:83`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm-deepseek/src/index.ts#L83) | `const catalogModel: z<DeepSeekCatalogModel> = z.object({` |
| `resolveApiKey` | `const` | [`packages/llm/llm-deepseek/src/index.ts:225`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm-deepseek/src/index.ts#L225) | `const resolveApiKey = async (connection: ResolvedDeepSeekOptions): Promise<string> => {` |
| `discoverModels` | `function` | [`packages/llm/llm-pi-ai/src/discovery.ts:195`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm-pi-ai/src/discovery.ts#L195) | `export async function discoverModels(` |
| `resolveApiKey` | `const` | [`packages/llm/llm-pi-ai/src/index.ts:175`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm-pi-ai/src/index.ts#L175) | `const resolveApiKey = async (` |
| `routeAuth` | `function` | [`packages/llm/llm-pi-ai/src/provider.ts:131`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm-pi-ai/src/provider.ts#L131) | `function routeAuth(spec: ProviderSpec, catalog: Provider \| undefined): Provider['auth'] {` |
| `normalizeApiKey` | `function` | [`packages/llm/llm/src/api-key.ts:36`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/src/api-key.ts#L36) | `export function normalizeApiKey(raw: string): ApiKeyCheck {` |
| `INVALID_CREDENTIAL_CODE` | `const` | [`packages/llm/llm/src/error.ts:48`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/src/error.ts#L48) | `export const INVALID_CREDENTIAL_CODE = 'INVALID_CREDENTIAL'` |

### Tests and executable evidence

- [`packages/llm/llm/tests/api-key.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/tests/api-key.spec.ts) — The source note names this file directly. A test under the owning area exercises or imports `sk-`.
- [`examples/headless-agent/tests/headless.snapshot.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/examples/headless-agent/tests/headless.snapshot.ts) — The source note names this file directly.
- [`packages/llm/llm/tests/service.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/tests/service.spec.ts) — A test under the owning area exercises or imports `LlmError`.
- [`packages/llm/llm/tests/topology.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/tests/topology.spec.ts) — A test under the owning area exercises or imports `LlmError`. A test under the owning area exercises or imports `discoverModels`.
- [`packages/llm/llm/tests/retry-policy.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/tests/retry-policy.spec.ts) — A test under the owning area exercises or imports `TRANSPORT`.
- [`packages/llm/llm-deepseek/tests/sse.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm-deepseek/tests/sse.spec.ts) — A test under the owning area exercises or imports `LlmError`.
- [`packages/llm/llm-pi-ai/tests/catalog.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm-pi-ai/tests/catalog.spec.ts) — A test under the owning area exercises or imports `Bearer`. A test under the owning area exercises or imports `apiKeyEnv`.
- [`packages/llm/llm-pi-ai/tests/adapter.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm-pi-ai/tests/adapter.spec.ts) — A test under the owning area exercises or imports `Bearer`. A test under the owning area exercises or imports `apiKeyEnv`.
- Source verification intent: packages/llm/llm/tests/api-key.spec.ts drives normalizeApiKey and assertUsableApiKey over the whole input table --- empty, whitespace-only, padded, interior-space, C0 control, emoji, CJK, full-width, latin-1, and the printable-ASCII boundary --- and pins that a refusal carries INVALID_CREDENTIAL and no part of the key. packages/llm/llm-deepseek/tests/ covers the stored-credential path end to end in dynamic-config.spec.ts, through the real credentials seam rather than a stub. packages/llm/llm-pi-ai/tests/ covers the discovery probe, including that a probe with no key sends no authorization header.

## How to read the implementation

1. Start with [`packages/llm/llm/src/attribution.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/src/attribution.ts) because it has the strongest evidence link to the note.
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
- **`rust/tokio-cancellation`** — Thread one cancellation token through async work, close owned resources before reuse, and test cancellation at every await boundary.
- **`rust/trait-seam`** — Use a small trait at the capability boundary; keep policy and model-facing schemas outside the backend implementation.

### Smallest safe implementation slice

1. Express the semantic contract as Rust types and one owning boundary.
2. Add the smallest adapter or service needed by the existing loop; do not add a new model-facing tool unless the four-tool surface cannot express the job.
3. Record durable evidence before deriving summaries or UI state.
4. Add one happy-path test, one interruption test, and one replay/restart test where state survives a turn.
5. Keep the patch reversible until the source note's strongest failure case is proven in the Rust harness.

## Search handles

- Tags: `class/bug-fix`, `concern/boundary`, `concern/discovery-routing`, `concern/evidence`, `concern/human-control`, `concern/lifecycle`, `concern/ownership`, `concern/schema-types`, `domain/build-release`, `domain/configuration`, `domain/extensions`, `domain/llm`, `domain/security`, `domain/shell-terminal`, `domain/storage`, `domain/testing`, `domain/ui-interaction`, `domain/web-retrieval`, `lifecycle/implemented`, `mechanism/adapter`, `mechanism/policy`, `mechanism/projection`
- Aliases: `empty`, `CustomProviderCard`, `modelFailure`, `validateDeepSeekModels`, `ProviderEditor`, `stringAt`, `apiKeyFailure`, `length`, `catalogModel`, `resolveApiKey`, `discoverModels`, `routeAuth`, `normalizeApiKey`, `INVALID_CREDENTIAL_CODE`
- Regex: `(?i)(empty|CustomProviderCard|modelFailure|validateDeepSeekModels|ProviderEditor|stringAt|apiKeyFailure|length)`

```bash
rg -n --pcre2 "(?i)(empty|CustomProviderCard|modelFailure|validateDeepSeekModels|ProviderEditor|stringAt|apiKeyFailure|length)" source-deepseek-harness
rg -l --fixed-strings "class/bug-fix" 03-agent-context
```

## Connected notes

- **`shares-code-with`** — [0516. Migrate simple unary API Proxy calls to business Remote services](0516-migrate-simple-unary-api-proxy-calls-to-business-remote-services.md): Shares source implementation: `packages/llm/llm/src/error.ts`, `packages/llm/llm/src/index.ts`.
- **`shares-code-with`** — [0016. Bounded recovery for transient LLM request failures](0016-bounded-recovery-for-transient-llm-request-failures.md): Shares source implementation: `packages/llm/llm/src/error.ts`, `packages/llm/llm/src/index.ts`.
- **`shares-code-with`** — [0017. Mandatory `User-Agent` attribution for provider requests](0017-mandatory-user-agent-attribution-for-provider-requests.md): Shares source implementation: `packages/llm/llm-deepseek/src/adapter.ts`, `packages/llm/llm/src/attribution.ts`.
- **`shares-code-with`** — [0018. Web capability seam - stable tools over multiple providers](0018-web-capability-seam-stable-tools-over-multiple-providers.md): Shares source implementation: `packages/llm/llm/src/error.ts`, `packages/llm/llm/src/index.ts`.
- **`shares-code-with`** — [0500. Keyless browser e2e lane for the web GUI](0500-keyless-browser-e2e-lane-for-the-web-gui.md): Shares source implementation: `packages/llm/llm/src/index.ts`, `packages/llm/llm/src/types.ts`.
- **`shares-code-with`** — [0010. Filesystem capability seam --- ctx.fs, local backend, and model-facing filesystem tools](0010-filesystem-capability-seam-ctx-fs-local-backend-and-model-facing-filesys.md): Shares source implementation: `packages/llm/llm/src/error.ts`, `packages/llm/llm/src/index.ts`.
- **`shares-code-with`** — [0133. Compaction as a capability seam (abstract contract + basic backend)](0133-compaction-as-a-capability-seam-abstract-contract-basic-backend.md): Shares source implementation: `packages/llm/llm/src/index.ts`, `packages/llm/llm/src/types.ts`.
- **`shares-code-with`** — [0131. Code Mode --- the model writes TypeScript against the tool registry](0131-code-mode-the-model-writes-typescript-against-the-tool-registry.md): Shares source implementation: `packages/llm/llm/src/index.ts`, `packages/llm/llm/src/types.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0346-validate-api-key-format-before-it-reaches-an-http-header.md`.
