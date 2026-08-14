---
id: "dsh-note-0097"
title: "Declaring a provider from the Models page"
status: "implemented"
class: "architecture"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/architecture/2026-08-04-declaring-a-provider-from-the-models-page.md"
implementation_evidence: "high"
target_anchor: "sandbox capability boundary"
tags:
  - "class/architecture"
  - "concern/boundary"
  - "concern/cancellation-timeout"
  - "concern/compatibility"
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
  - "domain/llm"
  - "domain/protocols"
  - "domain/security"
  - "domain/session-state"
  - "domain/testing"
  - "domain/web-retrieval"
  - "lifecycle/implemented"
  - "mechanism/adapter"
  - "mechanism/policy"
aliases:
  - "mutate"
  - "CustomProviderCard"
  - "ModelListEditor"
  - "ProviderEditor"
  - "protocolChoices"
  - "input"
  - "models"
  - "api"
  - "displayName"
  - "supportedProtocols"
  - "provider"
  - "yaml"
  - "set"
  - "$DSH_HOME/settings.yaml"
search_regex: "(?i)(mutate|CustomProviderCard|ModelListEditor|ProviderEditor|protocolChoices|input|models|displayName)"
---

# 0097. Declaring a provider from the Models page — implementation context

## Open this when

The two layers below made a pi-ai route a declaration and gave the host a way to interrogate a draft endpoint. Neither reached a person who does not edit YAML: the Models page still offered one API-key field per provider and a fold with a base URL, so adding a gateway meant opening $DSH_HOME/settings.yaml and knowing the profile shape, and correcting a stale context window meant the same. The capability existed and the surface did not expose it. Two things were missing, and they are not the same shape. Editing an existing route's models is a field on a card that already exists.

## Source decision

The model list is a component shared by both flows; the create is its own card. ModelListEditor edits a profile's models array --- one row per model with id, display name, context window, and output cap --- and owns the fetch action. An empty list means "serve this route's built-in catalog", so a row is only ever added deliberately; clearing an optional field drops it rather than storing a value the schema would reject, and a capacity that is not a positive integer is not stored at all.

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/architecture/2026-08-04-declaring-a-provider-from-the-models-page.md](../02-notes/implemented/architecture/2026-08-04-declaring-a-provider-from-the-models-page.md)
- Pinned source: [.agents/notes/implemented/architecture/2026-08-04-declaring-a-provider-from-the-models-page.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/architecture/2026-08-04-declaring-a-provider-from-the-models-page.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`packages/llm/llm-pi-ai/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm-pi-ai/src/index.ts) | package entry point | Core file in the package named by the note: `packages/llm/llm-pi-ai`. | `named-package-member` |
| [`packages/llm/llm-pi-ai/src/adapter.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm-pi-ai/src/adapter.ts) | provider/backend adapter | Core file in the package named by the note: `packages/llm/llm-pi-ai`. Defines `models`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/llm/llm-pi-ai/src/catalog.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm-pi-ai/src/catalog.ts) | runtime implementation | Core file in the package named by the note: `packages/llm/llm-pi-ai`. Defines `models`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/llm/llm-deepseek/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm-deepseek/src/index.ts) | package entry point | Core file in the package named by the note: `packages/llm/llm-deepseek`. | `named-package-member` |
| [`packages/llm/llm-deepseek/src/types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm-deepseek/src/types.ts) | public types and contract | Core file in the package named by the note: `packages/llm/llm-deepseek`. | `named-package-member` |
| [`packages/llm/llm-pi-ai/src/provider.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm-pi-ai/src/provider.ts) | provider/backend adapter | Core file in the package named by the note: `packages/llm/llm-pi-ai`. Defines `supportedProtocols`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/llm/llm-pi-ai/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm-pi-ai/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/llm/llm-pi-ai`. | `named-package-member` |
| [`packages/llm/llm-pi-ai/src/discovery.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm-pi-ai/src/discovery.ts) | runtime implementation | Core file in the package named by the note: `packages/llm/llm-pi-ai`. Defines `models`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/llm/llm-deepseek/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm-deepseek/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/llm/llm-deepseek`. | `named-package-member` |
| [`packages/core/agent-default-model/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent-default-model/src/index.ts) | package entry point | Core file in the package named by the note: `packages/core/agent-default-model`. | `named-package-member` |
| [`packages/core/agent-default-model/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent-default-model/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/core/agent-default-model`. | `named-package-member` |
| [`packages/llm/llm-pi-ai/src/config.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm-pi-ai/src/config.ts) | runtime implementation | Defines `displayName`, a construct named by the note. | `symbol-definition` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `mutate` | `const` | [`packages/client/runtime/src/client/contract/store.ts:220`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/runtime/src/client/contract/store.ts#L220) | `const mutate = decl.actions[key] as (draft: T, ...params: unknown[]) => void` |
| `CustomProviderCard` | `function` | [`packages/client/ui-settings-models/src/client/CustomProviderCard.tsx:76`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-settings-models/src/client/CustomProviderCard.tsx#L76) | `export function CustomProviderCard(props: CustomProviderCardProps): ReactNode {` |
| `ModelListEditor` | `function` | [`packages/client/ui-settings-models/src/client/ModelListEditor.tsx:161`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-settings-models/src/client/ModelListEditor.tsx#L161) | `export function ModelListEditor(props: ModelListEditorProps): ReactNode {` |
| `ProviderEditor` | `function` | [`packages/client/ui-settings-models/src/client/ProviderEditor.tsx:145`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-settings-models/src/client/ProviderEditor.tsx#L145) | `export function ProviderEditor(props: ProviderEditorProps): ReactNode {` |
| `protocolChoices` | `function` | [`packages/client/ui-settings-models/src/client/store.ts:81`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-settings-models/src/client/store.ts#L81) | `export function protocolChoices(namespace: SettingsNamespaceView \| undefined): string[] {` |
| `input` | `const` | [`packages/fs/tool-fs/src/edit.ts:113`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/tool-fs/src/edit.ts#L113) | `const input = parseEditArgs(args)` |
| `models` | `const` | [`packages/llm/llm-pi-ai/src/adapter.ts:202`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm-pi-ai/src/adapter.ts#L202) | `const models: MutableModels = createModels()` |
| `models` | `const` | [`packages/llm/llm-pi-ai/src/catalog.ts:171`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm-pi-ai/src/catalog.ts#L171) | `const models = getBuiltinModels(provider as BuiltinProvider) as Model<Api>[]` |
| `models` | `const` | [`packages/llm/llm-pi-ai/src/catalog.ts:492`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm-pi-ai/src/catalog.ts#L492) | `const models = entries.map((entry) => {` |
| `api` | `const` | [`packages/llm/llm-pi-ai/src/catalog.ts:497`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm-pi-ai/src/catalog.ts#L497) | `const api = request.api ?? base?.api ?? routeApi` |
| `displayName` | `const` | [`packages/llm/llm-pi-ai/src/config.ts:338`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm-pi-ai/src/config.ts#L338) | `const displayName = source.displayName ?? provider` |
| `models` | `const` | [`packages/llm/llm-pi-ai/src/discovery.ts:146`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm-pi-ai/src/discovery.ts#L146) | `const models: LlmDiscoveredModel[] = []` |
| `api` | `const` | [`packages/llm/llm-pi-ai/src/discovery.ts:225`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm-pi-ai/src/discovery.ts#L225) | `const api = request.api ?? 'openai-completions'` |
| `supportedProtocols` | `function` | [`packages/llm/llm-pi-ai/src/provider.ts:61`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm-pi-ai/src/provider.ts#L61) | `export function supportedProtocols(): readonly string[] {` |
| `provider` | `const` | [`packages/llm/llm/src/index.ts:632`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/src/index.ts#L632) | `const provider = registration.provider.id` |
| `yaml` | `const` | [`packages/skill/skill-filesystem/src/index.ts:917`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/skill/skill-filesystem/src/index.ts#L917) | `const yaml = raw.slice(start, closing.start)` |

### Tests and executable evidence

- [`apps/web/tests/models-settings.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/models-settings.e2e.ts) — The source note names this file directly.
- [`packages/client/ui-settings-models/tests/provider-form.client.spec.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-settings-models/tests/provider-form.client.spec.tsx) — The source note names this file directly. A test under the owning area exercises or imports `CustomProviderCard`.
- [`packages/llm/llm-pi-ai/tests/catalog.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm-pi-ai/tests/catalog.spec.ts) — A test under the owning area exercises or imports `supportedProtocols`. A test under the owning area exercises or imports `settingsPath`.
- [`packages/llm/llm-pi-ai/tests/adapter.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm-pi-ai/tests/adapter.spec.ts) — A test under the owning area exercises or imports `displayName`.
- [`packages/llm/llm-deepseek/tests/adapter.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm-deepseek/tests/adapter.spec.ts) — A test under the owning area exercises or imports `settingsPath`. A test under the owning area exercises or imports `displayName`.
- [`packages/llm/llm-pi-ai/tests/dynamic-config.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm-pi-ai/tests/dynamic-config.spec.ts) — A test under the owning area exercises or imports `settingsPath`. A test under the owning area exercises or imports `displayName`.
- [`packages/llm/llm-pi-ai/tests/loader-composition.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm-pi-ai/tests/loader-composition.spec.ts) — A test under the owning area exercises or imports `settingsPath`.
- [`packages/llm/llm-deepseek/tests/loader-composition.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm-deepseek/tests/loader-composition.spec.ts) — A test under the owning area exercises or imports `settingsPath`.
- Source verification intent: packages/client/ui-settings-models/tests/provider-form.client.spec.tsx drives the rendered page over a scripted wire face: adding, editing, and removing rows; a cleared optional field leaving the profile and a non-integer capacity never entering it; the interrogation carrying the edited endpoint, the unsaved key, and the profile's protocol; the picker's default selection, toggling, cancel, and adopt-keeps-tuned-rows; the empty, refused, and rejected-transport paths; the create writing one profile plus its credential; every gate on the create button; and the read-only posture.

## How to read the implementation

1. Start with [`packages/llm/llm-pi-ai/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm-pi-ai/src/index.ts) because it has the strongest evidence link to the note.
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

- Tags: `class/architecture`, `concern/boundary`, `concern/cancellation-timeout`, `concern/compatibility`, `concern/discovery-routing`, `concern/evidence`, `concern/lifecycle`, `concern/ownership`, `concern/recovery`, `concern/schema-types`, `concern/simplification`, `domain/build-release`, `domain/configuration`, `domain/context`, `domain/extensions`, `domain/llm`, `domain/protocols`, `domain/security`, `domain/session-state`, `domain/testing`, `domain/web-retrieval`, `lifecycle/implemented`, `mechanism/adapter`, `mechanism/policy`
- Aliases: `mutate`, `CustomProviderCard`, `ModelListEditor`, `ProviderEditor`, `protocolChoices`, `input`, `models`, `api`, `displayName`, `supportedProtocols`, `provider`, `yaml`, `set`, `$DSH_HOME/settings.yaml`
- Regex: `(?i)(mutate|CustomProviderCard|ModelListEditor|ProviderEditor|protocolChoices|input|models|displayName)`

```bash
rg -n --pcre2 "(?i)(mutate|CustomProviderCard|ModelListEditor|ProviderEditor|protocolChoices|input|models|displayName)" source-deepseek-harness
rg -l --fixed-strings "class/architecture" 03-agent-context
```

## Connected notes

- **`source-link`** — [0094. pi-ai routes are declared providers, not catalog lookups](0094-pi-ai-routes-are-declared-providers-not-catalog-lookups.md): The source note links to this decision directly.
- **`source-link`** — [0098. Interrogating a draft provider endpoint](0098-interrogating-a-draft-provider-endpoint.md): The source note links to this decision directly.
- **`shares-code-with`** — [0087. the web configuration plane](0087-the-web-configuration-plane.md): Shares source implementation: `apps/web/tests/models-settings.e2e.ts`, `packages/llm/llm-deepseek/src/index.ts`.
- **`shares-code-with`** — [0039. Advisory LLM catalogs and per-session ACP model selection](0039-advisory-llm-catalogs-and-per-session-acp-model-selection.md): Shares source implementation: `packages/llm/llm-deepseek/src/index.ts`, `packages/llm/llm-deepseek/src/types.ts`.
- **`shares-code-with`** — [0008. Two LLM adapters as a design-verification twin](0008-two-llm-adapters-as-a-design-verification-twin.md): Shares source implementation: `packages/llm/llm-deepseek/src/index.ts`, `packages/llm/llm-deepseek/src/types.ts`.
- **`shares-code-with`** — [0128. A pi-ai model declares its own input modalities, and undeclared means text](0128-a-pi-ai-model-declares-its-own-input-modalities-and-undeclared-means-tex.md): Shares source implementation: `packages/llm/llm-pi-ai/src/adapter.ts`, `packages/llm/llm-pi-ai/src/catalog.ts`.
- **`shares-code-with`** — [0017. Mandatory `User-Agent` attribution for provider requests](0017-mandatory-user-agent-attribution-for-provider-requests.md): Shares source implementation: `packages/llm/llm-deepseek/src/index.ts`, `packages/llm/llm-deepseek/src/types.ts`.
- **`shares-code-with`** — [0077. request-level LLM configuration and the credential seam](0077-request-level-llm-configuration-and-the-credential-seam.md): Shares source implementation: `packages/llm/llm-deepseek/src/index.ts`, `packages/llm/llm-pi-ai/src/adapter.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0097-declaring-a-provider-from-the-models-page.md`.
