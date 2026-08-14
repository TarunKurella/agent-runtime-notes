---
id: "dsh-note-0371"
title: "First-run readiness reads every provider, and the setup card closes"
status: "implemented"
class: "bug-fix"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/bug-fix/2026-08-12-onboarding-reads-every-provider.md"
implementation_evidence: "medium"
target_anchor: "sandbox capability boundary"
tags:
  - "class/bug-fix"
  - "concern/boundary"
  - "concern/cancellation-timeout"
  - "concern/discovery-routing"
  - "concern/evidence"
  - "concern/lifecycle"
  - "concern/ownership"
  - "domain/build-release"
  - "domain/configuration"
  - "domain/extensions"
  - "domain/llm"
  - "domain/observability"
  - "domain/security"
  - "domain/session-state"
  - "domain/storage"
  - "domain/testing"
  - "lifecycle/implemented"
  - "mechanism/adapter"
  - "mechanism/policy"
  - "mechanism/projection"
  - "mechanism/registry"
aliases:
  - "needsSetup"
  - "announceSaved"
  - "closeEditor"
  - "closeSetup"
  - "providerUsable"
  - "onboardingReadiness"
  - "active"
  - "configured"
  - "models"
  - "deepseek-official"
  - "entry.active"
  - "deepSeekReadiness"
  - "provider-ready"
  - "settings-unavailable"
search_regex: "(?i)(needsSetup|announceSaved|closeEditor|closeSetup|providerUsable|onboardingReadiness|active|configured)"
---

# 0371. First-run readiness reads every provider, and the setup card closes — implementation context

## Open this when

The first-run step and the Models page both asked one question --- is deepseek-official's credential stored? --- of a join that describes every provider. Two defects followed from that single reading. A user who configured some other provider (a pi-ai gateway, a self-hosted route) and never wanted the official DeepSeek endpoint was taken over by the full-screen credential prompt on every blank session, with a working model already selected in the composer behind it.

## Source decision

One predicate answers what both surfaces actually need. providerUsable(row) is true when the route is registered with the adapter registry (entry.active) and whatever credential its resolved profile names is stored; a profile naming no reference authenticates through the provider's own path, as does a live route with no settings address, so neither owes this page a key. onboardingReadiness (renamed from deepSeekReadiness, which no longer describes what it reads) returns provider-ready as soon as any joined row is usable.

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/bug-fix/2026-08-12-onboarding-reads-every-provider.md](../02-notes/implemented/bug-fix/2026-08-12-onboarding-reads-every-provider.md)
- Pinned source: [.agents/notes/implemented/bug-fix/2026-08-12-onboarding-reads-every-provider.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/bug-fix/2026-08-12-onboarding-reads-every-provider.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`packages/llm/llm-deepseek/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm-deepseek/src/index.ts) | package entry point | Core file in the package named by the note: `packages/llm/llm-deepseek`. | `named-package-member` |
| [`packages/llm/llm-deepseek/src/types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm-deepseek/src/types.ts) | public types and contract | Core file in the package named by the note: `packages/llm/llm-deepseek`. | `named-package-member` |
| [`packages/llm/llm-deepseek/src/adapter.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm-deepseek/src/adapter.ts) | provider/backend adapter | Core file in the package named by the note: `packages/llm/llm-deepseek`. Defines `configured`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/llm/llm-deepseek/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm-deepseek/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/llm/llm-deepseek`. | `named-package-member` |
| [`packages/llm/llm-deepseek`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm-deepseek) | package or module directory | The note names this package or capability. | `named-package` |
| [`packages/llm/llm/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/src/index.ts) | package entry point | Defines `models`, a construct named by the note. | `symbol-definition` |
| [`packages/core/scope/src/store.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/scope/src/store.ts) | runtime implementation | Defines `active`, a construct named by the note. | `symbol-definition` |
| [`packages/client/ui-settings-models/src/client/store.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-settings-models/src/client/store.ts) | runtime implementation | Defines `providerUsable`, a construct named by the note. Defines `onboardingReadiness`, a construct named by the note. | `symbol-definition` |
| [`packages/client/ui-settings-models/src/client/ModelsSection.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-settings-models/src/client/ModelsSection.tsx) | runtime implementation | Defines `needsSetup`, a construct named by the note. Defines `closeSetup`, a construct named by the note. | `symbol-definition` |
| [`packages/llm/llm-deepseek/README.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm-deepseek/README.md) | package contract and examples | Core file in the package named by the note: `packages/llm/llm-deepseek`. | `named-package-member` |
| [`packages/llm/llm-deepseek/package.json`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm-deepseek/package.json) | composition and configuration | Core file in the package named by the note: `packages/llm/llm-deepseek`. | `named-package-member` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `needsSetup` | `function` | [`packages/client/ui-settings-models/src/client/ModelsSection.tsx:128`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-settings-models/src/client/ModelsSection.tsx#L128) | `export function needsSetup(row: ProviderRow, anyUsable: boolean): boolean {` |
| `announceSaved` | `const` | [`packages/client/ui-settings-models/src/client/ModelsSection.tsx:189`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-settings-models/src/client/ModelsSection.tsx#L189) | `const announceSaved = (target: ProviderIdentity): void => {` |
| `closeEditor` | `const` | [`packages/client/ui-settings-models/src/client/ModelsSection.tsx:196`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-settings-models/src/client/ModelsSection.tsx#L196) | `const closeEditor = (changed: boolean, target: ProviderIdentity): void => {` |
| `closeSetup` | `const` | [`packages/client/ui-settings-models/src/client/ModelsSection.tsx:210`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-settings-models/src/client/ModelsSection.tsx#L210) | `const closeSetup = (changed: boolean, target: ProviderIdentity): void => {` |
| `providerUsable` | `function` | [`packages/client/ui-settings-models/src/client/store.ts:202`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-settings-models/src/client/store.ts#L202) | `export function providerUsable(row: ProviderRow): boolean {` |
| `onboardingReadiness` | `function` | [`packages/client/ui-settings-models/src/client/store.ts:234`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-settings-models/src/client/store.ts#L234) | `export function onboardingReadiness(state: ModelsSettingsState): OnboardingReadiness {` |
| `active` | `let` | [`packages/core/scope/src/store.ts:47`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/scope/src/store.ts#L47) | `let active = true` |
| `configured` | `const` | [`packages/llm/llm-deepseek/src/adapter.ts:181`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm-deepseek/src/adapter.ts#L181) | `const configured = connection.models.find(entry => entry.id === model)` |
| `models` | `const` | [`packages/llm/llm/src/index.ts:547`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/src/index.ts#L547) | `const models: LlmDiscoveredModel[] = []` |

### Tests and executable evidence

- [`packages/client/ui-settings-models/tests/readiness.client.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-settings-models/tests/readiness.client.spec.ts) — A test under the owning area exercises or imports `providerUsable`. A test under the owning area exercises or imports `onboardingReadiness`.
- [`packages/client/ui-settings-models/tests/components.client.spec.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-settings-models/tests/components.client.spec.tsx) — A test under the owning area exercises or imports `needsSetup`.
- Source verification intent: Package tests pin providerUsable over the four join states and onboardingReadiness over both the new gate and every surviving diagnostic; the section tests cover the first-run posture, the plain-row posture, and the cancel that collapses the setup card while the add card keeps its draft. The onboarding-usable-provider web e2e lane replays the whole scenario through the real wire: cancel with both cards open, configure minimax-cn instead, reload, and find no takeover --- with one aria golden of the dismissed state.

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

- Tags: `class/bug-fix`, `concern/boundary`, `concern/cancellation-timeout`, `concern/discovery-routing`, `concern/evidence`, `concern/lifecycle`, `concern/ownership`, `domain/build-release`, `domain/configuration`, `domain/extensions`, `domain/llm`, `domain/observability`, `domain/security`, `domain/session-state`, `domain/storage`, `domain/testing`, `lifecycle/implemented`, `mechanism/adapter`, `mechanism/policy`, `mechanism/projection`, `mechanism/registry`
- Aliases: `needsSetup`, `announceSaved`, `closeEditor`, `closeSetup`, `providerUsable`, `onboardingReadiness`, `active`, `configured`, `models`, `deepseek-official`, `entry.active`, `deepSeekReadiness`, `provider-ready`, `settings-unavailable`
- Regex: `(?i)(needsSetup|announceSaved|closeEditor|closeSetup|providerUsable|onboardingReadiness|active|configured)`

```bash
rg -n --pcre2 "(?i)(needsSetup|announceSaved|closeEditor|closeSetup|providerUsable|onboardingReadiness|active|configured)" source-deepseek-harness
rg -l --fixed-strings "class/bug-fix" 03-agent-context
```

## Connected notes

- **`shares-code-with`** — [0213. official DeepSeek first-run credential setup](0213-official-deepseek-first-run-credential-setup.md): Shares source implementation: `packages/llm/llm-deepseek`, `packages/llm/llm-deepseek/src/index.ts`.
- **`shares-code-with`** — [0291. DeepSeek request user and session identity headers](0291-deepseek-request-user-and-session-identity-headers.md): Shares source implementation: `packages/llm/llm-deepseek`, `packages/llm/llm-deepseek/src/adapter.ts`.
- **`shares-code-with`** — [0017. Mandatory `User-Agent` attribution for provider requests](0017-mandatory-user-agent-attribution-for-provider-requests.md): Shares source implementation: `packages/llm/llm-deepseek/src/adapter.ts`, `packages/llm/llm-deepseek/src/index.ts`.
- **`shares-code-with`** — [0500. Keyless browser e2e lane for the web GUI](0500-keyless-browser-e2e-lane-for-the-web-gui.md): Shares source implementation: `packages/llm/llm-deepseek/src/index.ts`, `packages/llm/llm-deepseek/src/types.ts`.
- **`shares-code-with`** — [0094. pi-ai routes are declared providers, not catalog lookups](0094-pi-ai-routes-are-declared-providers-not-catalog-lookups.md): Shares source implementation: `packages/llm/llm-deepseek/src/adapter.ts`, `packages/llm/llm-deepseek/src/index.ts`.
- **`shares-code-with`** — [0495. ACP snapshot tests --- record-once / replay-deterministic](0495-acp-snapshot-tests-record-once-replay-deterministic.md): Shares source implementation: `packages/llm/llm-deepseek/src/index.ts`, `packages/llm/llm-deepseek/src/invariant.ts`.
- **`shares-code-with`** — [0501. Scriptable LLM wire fault server](0501-scriptable-llm-wire-fault-server.md): Shares source implementation: `packages/llm/llm-deepseek/src/index.ts`, `packages/llm/llm-deepseek/src/invariant.ts`.
- **`shares-code-with`** — [0097. Declaring a provider from the Models page](0097-declaring-a-provider-from-the-models-page.md): Shares source implementation: `packages/llm/llm-deepseek/src/index.ts`, `packages/llm/llm-deepseek/src/types.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0371-first-run-readiness-reads-every-provider-and-the-setup-card-closes.md`.
