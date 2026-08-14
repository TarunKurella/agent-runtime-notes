---
id: "dsh-note-0213"
title: "official DeepSeek first-run credential setup"
status: "implemented"
class: "feature"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/feature/2026-07-30-deepseek-onboarding-credential-setup.md"
implementation_evidence: "medium"
target_anchor: "sandbox capability boundary"
tags:
  - "class/feature"
  - "concern/boundary"
  - "concern/cancellation-timeout"
  - "concern/concurrency"
  - "concern/discovery-routing"
  - "concern/evidence"
  - "concern/lifecycle"
  - "concern/ownership"
  - "concern/recovery"
  - "concern/schema-types"
  - "domain/build-release"
  - "domain/configuration"
  - "domain/extensions"
  - "domain/llm"
  - "domain/observability"
  - "domain/security"
  - "domain/session-state"
  - "domain/shell-terminal"
  - "domain/storage"
  - "domain/testing"
  - "domain/web-retrieval"
  - "lifecycle/implemented"
  - "mechanism/adapter"
  - "mechanism/event-sourcing"
  - "mechanism/policy"
  - "mechanism/projection"
aliases:
  - "openSection"
  - "ProviderEditor"
  - "complete"
  - "apiKeyEnv"
  - "deepseek-official"
  - "llm-deepseek"
  - "ui-settings-models"
  - "llm.providers"
  - "settings.describe"
  - "credentials.describe"
  - "ui-settings"
  - "settings.onboarding"
  - "slots.inject"
  - "credentials.set"
search_regex: "(?i)(openSection|ProviderEditor|complete|apiKeyEnv|deepseek\\-official|llm\\-deepseek|ui\\-settings\\-models|llm\\.providers)"
---

# 0213. official DeepSeek first-run credential setup — implementation context

## Open this when

The web configuration plane makes provider settings and credentials live-editable, but a first-time user still lands on the empty conversation Hero without an actionable explanation when the shipped deepseek-official route has no credential. The Models page can repair that state, yet requiring the user to discover it weakens onboarding. A prompt must not confuse a missing credential with a missing adapter: the browser can store a value for an existing credential reference, but it cannot dynamically mount the llm-deepseek Cordis plugin.

## Source decision

One readiness projection owns both Models and onboarding facts. ui-settings-models keeps a single store that joins llm.providers({}), redacted settings.describe({}), and batched credentials.describe({refs}). The onboarding projection selects the deepseek-official configurable-provider entry owned by the llm-deepseek namespace and empty settings path, reads the effective apiKeyEnv, and evaluates the matching credential descriptor. A live route with the same provider id but no matching configurable-provider declaration is adapter-absent for onboarding.

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/feature/2026-07-30-deepseek-onboarding-credential-setup.md](../02-notes/implemented/feature/2026-07-30-deepseek-onboarding-credential-setup.md)
- Pinned source: [.agents/notes/implemented/feature/2026-07-30-deepseek-onboarding-credential-setup.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/feature/2026-07-30-deepseek-onboarding-credential-setup.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`packages/llm/llm-deepseek/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm-deepseek/src/index.ts) | package entry point | Core file in the package named by the note: `packages/llm/llm-deepseek`. | `named-package-member` |
| [`packages/llm/llm-deepseek/src/types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm-deepseek/src/types.ts) | public types and contract | Core file in the package named by the note: `packages/llm/llm-deepseek`. | `named-package-member` |
| [`packages/client/ui-settings/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-settings/src/index.ts) | package entry point | Core file in the package named by the note: `packages/client/ui-settings`. | `named-package-member` |
| [`packages/llm/llm-deepseek/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm-deepseek/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/llm/llm-deepseek`. | `named-package-member` |
| [`packages/client/ui-settings/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-settings/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/client/ui-settings`. | `named-package-member` |
| [`packages/client/ui-settings-models/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-settings-models/src/index.ts) | package entry point | Core file in the package named by the note: `packages/client/ui-settings-models`. | `named-package-member` |
| [`packages/client/ui-settings-models/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-settings-models/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/client/ui-settings-models`. | `named-package-member` |
| [`packages/client/ui-settings-models/src/client/ProviderEditor.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-settings-models/src/client/ProviderEditor.tsx) | provider/backend adapter | Core file in the package named by the note: `packages/client/ui-settings-models`. Defines `ProviderEditor`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/llm/llm-deepseek`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm-deepseek) | package or module directory | The note names this package or capability. | `named-package` |
| [`packages/client/ui-settings`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-settings) | package or module directory | The note names this package or capability. | `named-package` |
| [`packages/client/ui-settings-models`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-settings-models) | package or module directory | The note names this package or capability. | `named-package` |
| [`packages/jobs/tool-jobs/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/jobs/tool-jobs/src/index.ts) | package entry point | Defines `complete`, a construct named by the note. | `symbol-definition` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `openSection` | `const` | [`packages/client/ui-settings-general/src/client/SettingsRoot.tsx:113`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-settings-general/src/client/SettingsRoot.tsx#L113) | `const openSection = useCallback((id: string) => {` |
| `ProviderEditor` | `function` | [`packages/client/ui-settings-models/src/client/ProviderEditor.tsx:145`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-settings-models/src/client/ProviderEditor.tsx#L145) | `export function ProviderEditor(props: ProviderEditorProps): ReactNode {` |
| `complete` | `const` | [`packages/jobs/tool-jobs/src/index.ts:129`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/jobs/tool-jobs/src/index.ts#L129) | `const complete = \`${content}${suffix}\`` |
| `apiKeyEnv` | `const` | [`packages/web/web-search-deepseek/src/index.ts:96`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/web/web-search-deepseek/src/index.ts#L96) | `const apiKeyEnv = credentialRef(config.apiKeyEnv ?? DEFAULT_API_KEY_ENV)` |

### Tests and executable evidence

- [`packages/llm/llm-deepseek/tests/adapter.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm-deepseek/tests/adapter.spec.ts) — A test under the owning area exercises or imports `onboarding`.
- [`packages/client/ui-settings-models/tests/apply.client.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-settings-models/tests/apply.client.spec.ts) — A test under the owning area exercises or imports `llm-deepseek`. A test under the owning area exercises or imports `ui-settings-models`.
- [`packages/client/ui-settings-models/tests/store.client.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-settings-models/tests/store.client.spec.ts) — A test under the owning area exercises or imports `llm-deepseek`. A test under the owning area exercises or imports `apiKeyEnv`.
- [`packages/client/ui-settings-models/tests/readiness.client.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-settings-models/tests/readiness.client.spec.ts) — A test under the owning area exercises or imports `llm-deepseek`. A test under the owning area exercises or imports `apiKeyEnv`.
- [`packages/client/ui-settings-models/tests/components.client.spec.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-settings-models/tests/components.client.spec.tsx) — A test under the owning area exercises or imports `llm-deepseek`. A test under the owning area exercises or imports `apiKeyEnv`.
- [`packages/client/ui-settings-models/tests/provider-form.client.spec.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-settings-models/tests/provider-form.client.spec.tsx) — A test under the owning area exercises or imports `apiKeyEnv`.
- [`packages/client/ui-settings-models/tests/welcome-notice.client.spec.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-settings-models/tests/welcome-notice.client.spec.tsx) — A test under the owning area exercises or imports `openSection`.
- [`packages/client/ui-settings-general/tests/settings-root.client.spec.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-settings-general/tests/settings-root.client.spec.tsx) — A test under the owning area exercises or imports `openSection`.

## How to read the implementation

1. Start with [`packages/llm/llm-deepseek/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm-deepseek/src/index.ts) because it has the strongest evidence link to the note.
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

- Tags: `class/feature`, `concern/boundary`, `concern/cancellation-timeout`, `concern/concurrency`, `concern/discovery-routing`, `concern/evidence`, `concern/lifecycle`, `concern/ownership`, `concern/recovery`, `concern/schema-types`, `domain/build-release`, `domain/configuration`, `domain/extensions`, `domain/llm`, `domain/observability`, `domain/security`, `domain/session-state`, `domain/shell-terminal`, `domain/storage`, `domain/testing`, `domain/web-retrieval`, `lifecycle/implemented`, `mechanism/adapter`, `mechanism/event-sourcing`, `mechanism/policy`, `mechanism/projection`
- Aliases: `openSection`, `ProviderEditor`, `complete`, `apiKeyEnv`, `deepseek-official`, `llm-deepseek`, `ui-settings-models`, `llm.providers`, `settings.describe`, `credentials.describe`, `ui-settings`, `settings.onboarding`, `slots.inject`, `credentials.set`
- Regex: `(?i)(openSection|ProviderEditor|complete|apiKeyEnv|deepseek\-official|llm\-deepseek|ui\-settings\-models|llm\.providers)`

```bash
rg -n --pcre2 "(?i)(openSection|ProviderEditor|complete|apiKeyEnv|deepseek\\-official|llm\\-deepseek|ui\\-settings\\-models|llm\\.providers)" source-deepseek-harness
rg -l --fixed-strings "class/feature" 03-agent-context
```

## Connected notes

- **`source-link`** — [0087. the web configuration plane](0087-the-web-configuration-plane.md): The source note links to this decision directly.
- **`source-link`** — [0299. Shared-modal product onboarding](0299-shared-modal-product-onboarding.md): The source note links to this decision directly.
- **`shares-code-with`** — [0371. First-run readiness reads every provider, and the setup card closes](0371-first-run-readiness-reads-every-provider-and-the-setup-card-closes.md): Shares source implementation: `packages/llm/llm-deepseek`, `packages/llm/llm-deepseek/src/index.ts`.
- **`shares-code-with`** — [0217. Versioned GUI welcome onboarding](0217-versioned-gui-welcome-onboarding.md): Shares source implementation: `packages/client/ui-settings`, `packages/client/ui-settings-models/src/index.ts`.
- **`shares-code-with`** — [0291. DeepSeek request user and session identity headers](0291-deepseek-request-user-and-session-identity-headers.md): Shares source implementation: `packages/llm/llm-deepseek`, `packages/llm/llm-deepseek/src/index.ts`.
- **`shares-code-with`** — [0495. ACP snapshot tests --- record-once / replay-deterministic](0495-acp-snapshot-tests-record-once-replay-deterministic.md): Shares source implementation: `packages/llm/llm-deepseek/src/index.ts`, `packages/llm/llm-deepseek/src/invariant.ts`.
- **`shares-code-with`** — [0500. Keyless browser e2e lane for the web GUI](0500-keyless-browser-e2e-lane-for-the-web-gui.md): Shares source implementation: `packages/llm/llm-deepseek/src/index.ts`, `packages/llm/llm-deepseek/src/types.ts`.
- **`shares-code-with`** — [0501. Scriptable LLM wire fault server](0501-scriptable-llm-wire-fault-server.md): Shares source implementation: `packages/llm/llm-deepseek/src/index.ts`, `packages/llm/llm-deepseek/src/invariant.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0213-official-deepseek-first-run-credential-setup.md`.
