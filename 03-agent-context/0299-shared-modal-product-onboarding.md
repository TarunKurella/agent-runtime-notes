---
id: "dsh-note-0299"
title: "Shared-modal product onboarding"
status: "implemented"
class: "feature"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/feature/2026-08-13-shared-modal-product-onboarding.md"
implementation_evidence: "medium"
target_anchor: "sandbox capability boundary"
tags:
  - "class/feature"
  - "concern/boundary"
  - "concern/compatibility"
  - "concern/concurrency"
  - "concern/evidence"
  - "concern/ownership"
  - "concern/recovery"
  - "concern/schema-types"
  - "concern/simplification"
  - "domain/configuration"
  - "domain/extensions"
  - "domain/llm"
  - "domain/observability"
  - "domain/security"
  - "domain/shell-terminal"
  - "domain/storage"
  - "domain/testing"
  - "domain/ui-interaction"
  - "domain/web-retrieval"
  - "lifecycle/implemented"
aliases:
  - "Modal"
  - "OnboardingModal"
  - "ProviderEditor"
  - "yaml"
  - "set"
  - "ui-settings-models"
  - "welcome-notice"
  - "-100"
  - "deepseek-official"
  - "onboarding-copy.ts"
  - "ui-onboarding.welcomeNoticeVersion"
  - "credentials.set"
  - "settings.yaml"
  - ".credentials.yaml"
search_regex: "(?i)(Modal|OnboardingModal|ProviderEditor|yaml|ui\\-settings\\-models|welcome\\-notice|\\-100|deepseek\\-official)"
---

# 0299. Shared-modal product onboarding — implementation context

## Open this when

First-run onboarding mixed two interaction models: a viewport takeover for product context and a credential prompt that redirected users into Settings before they could enter a key. That made a short, ordered flow feel like two unrelated surfaces and left onboarding UI ownership split across packages. The product still needs a versioned testing-stage notice before provider setup, but restoring it must not add a second independent overlay or change the Host settings and credential boundaries.

## Source decision

One existing client Cordis plugin owns both shipped steps. ui-settings-models registers welcome-notice at order -100 and deepseek-official at order 0 in settings.onboarding. The shell continues to mount only the first incomplete entry, so the dialogs cannot stack. No additional client package or plugin row is introduced. Both steps share one modal component. OnboardingModal wraps the existing ui-primitives Modal, supplies the common title and content geometry, and owns #root inert for exactly the visible lifetime.

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/feature/2026-08-13-shared-modal-product-onboarding.md](../02-notes/implemented/feature/2026-08-13-shared-modal-product-onboarding.md)
- Pinned source: [.agents/notes/implemented/feature/2026-08-13-shared-modal-product-onboarding.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/feature/2026-08-13-shared-modal-product-onboarding.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`packages/client/ui-settings-models/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-settings-models/src/index.ts) | package entry point | Core file in the package named by the note: `packages/client/ui-settings-models`. | `named-package-member` |
| [`packages/client/ui-settings-models/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-settings-models/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/client/ui-settings-models`. | `named-package-member` |
| [`packages/client/ui-settings-models/src/client/ProviderEditor.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-settings-models/src/client/ProviderEditor.tsx) | provider/backend adapter | Core file in the package named by the note: `packages/client/ui-settings-models`. Defines `ProviderEditor`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/client/ui-settings-models/src/client/OnboardingModal.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-settings-models/src/client/OnboardingModal.tsx) | runtime implementation | Core file in the package named by the note: `packages/client/ui-settings-models`. Defines `OnboardingModal`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/client/ui-settings-models`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-settings-models) | package or module directory | The note names this package or capability. | `named-package` |
| [`packages/client/ui-primitives/src/Modal.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-primitives/src/Modal.tsx) | runtime implementation | Defines `Modal`, a construct named by the note. | `symbol-definition` |
| [`packages/skill/skill-filesystem/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/skill/skill-filesystem/src/index.ts) | package entry point | Defines `yaml`, a construct named by the note. | `symbol-definition` |
| [`packages/test-support/client-runtime/src/sessions.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/test-support/client-runtime/src/sessions.ts) | runtime implementation | Defines `set`, a construct named by the note. | `symbol-definition` |
| [`packages/client/ui-settings-models/README.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-settings-models/README.md) | package contract and examples | Core file in the package named by the note: `packages/client/ui-settings-models`. | `named-package-member` |
| [`packages/client/ui-settings-models/package.json`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-settings-models/package.json) | composition and configuration | Core file in the package named by the note: `packages/client/ui-settings-models`. | `named-package-member` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `Modal` | `function` | [`packages/client/ui-primitives/src/Modal.tsx:30`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-primitives/src/Modal.tsx#L30) | `export function Modal({` |
| `OnboardingModal` | `function` | [`packages/client/ui-settings-models/src/client/OnboardingModal.tsx:17`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-settings-models/src/client/OnboardingModal.tsx#L17) | `export function OnboardingModal({` |
| `ProviderEditor` | `function` | [`packages/client/ui-settings-models/src/client/ProviderEditor.tsx:145`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-settings-models/src/client/ProviderEditor.tsx#L145) | `export function ProviderEditor(props: ProviderEditorProps): ReactNode {` |
| `yaml` | `const` | [`packages/skill/skill-filesystem/src/index.ts:917`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/skill/skill-filesystem/src/index.ts#L917) | `const yaml = raw.slice(start, closing.start)` |
| `set` | `const` | [`packages/test-support/client-runtime/src/sessions.ts:52`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/test-support/client-runtime/src/sessions.ts#L52) | `const set = listeners.get(key) ?? new Set()` |

### Tests and executable evidence

- [`packages/client/ui-primitives/tests/atoms.client.spec.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-primitives/tests/atoms.client.spec.tsx) — A test under the owning area exercises or imports `Modal`.
- [`packages/client/ui-settings-models/tests/apply.client.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-settings-models/tests/apply.client.spec.ts) — A test under the owning area exercises or imports `ui-settings-models`. A test under the owning area exercises or imports `welcome-notice`.
- [`packages/client/ui-settings-models/tests/components.client.spec.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-settings-models/tests/components.client.spec.tsx) — A test under the owning area exercises or imports `ProviderEditor`.
- [`packages/client/ui-settings-models/tests/welcome-notice.client.spec.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-settings-models/tests/welcome-notice.client.spec.tsx) — A test under the owning area exercises or imports `welcome-notice`.

## How to read the implementation

1. Start with [`packages/client/ui-settings-models/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-settings-models/src/index.ts) because it has the strongest evidence link to the note.
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

- Tags: `class/feature`, `concern/boundary`, `concern/compatibility`, `concern/concurrency`, `concern/evidence`, `concern/ownership`, `concern/recovery`, `concern/schema-types`, `concern/simplification`, `domain/configuration`, `domain/extensions`, `domain/llm`, `domain/observability`, `domain/security`, `domain/shell-terminal`, `domain/storage`, `domain/testing`, `domain/ui-interaction`, `domain/web-retrieval`, `lifecycle/implemented`
- Aliases: `Modal`, `OnboardingModal`, `ProviderEditor`, `yaml`, `set`, `ui-settings-models`, `welcome-notice`, `-100`, `deepseek-official`, `onboarding-copy.ts`, `ui-onboarding.welcomeNoticeVersion`, `credentials.set`, `settings.yaml`, `.credentials.yaml`
- Regex: `(?i)(Modal|OnboardingModal|ProviderEditor|yaml|ui\-settings\-models|welcome\-notice|\-100|deepseek\-official)`

```bash
rg -n --pcre2 "(?i)(Modal|OnboardingModal|ProviderEditor|yaml|ui\\-settings\\-models|welcome\\-notice|\\-100|deepseek\\-official)" source-deepseek-harness
rg -l --fixed-strings "class/feature" 03-agent-context
```

## Connected notes

- **`source-link`** — [0493. Remove the first-run beta notice](0493-remove-the-first-run-beta-notice.md): The source note links to this decision directly.
- **`shares-code-with`** — [0217. Versioned GUI welcome onboarding](0217-versioned-gui-welcome-onboarding.md): Shares source implementation: `packages/client/ui-settings-models`, `packages/client/ui-settings-models/src/client/OnboardingModal.tsx`.
- **`shares-code-with`** — [0213. official DeepSeek first-run credential setup](0213-official-deepseek-first-run-credential-setup.md): Shares source implementation: `packages/client/ui-settings-models/src/client/ProviderEditor.tsx`, `packages/client/ui-settings-models/src/index.ts`.
- **`shares-code-with`** — [0349. onboarding takeover chrome moves into the step](0349-onboarding-takeover-chrome-moves-into-the-step.md): Shares source implementation: `packages/client/ui-settings-models`, `packages/client/ui-settings-models/src/index.ts`.
- **`shares-code-with`** — [0350. Recoverable provider credential lifecycle](0350-recoverable-provider-credential-lifecycle.md): Shares source implementation: `packages/skill/skill-filesystem/src/index.ts`, `packages/test-support/client-runtime/src/sessions.ts`.
- **`shares-code-with`** — [0192. Skill catalog hot refresh](0192-skill-catalog-hot-refresh.md): Shares source implementation: `packages/skill/skill-filesystem/src/index.ts`.
- **`shares-code-with`** — [0458. Plan-specific collaboration state](0458-plan-specific-collaboration-state.md): Shares source implementation: `packages/test-support/client-runtime/src/sessions.ts`.
- **`shares-code-with`** — [0080. Full client copy rollout onto the typed locale seat, and the non-translation boundary](0080-full-client-copy-rollout-onto-the-typed-locale-seat-and-the-non-translat.md): Shares source implementation: `packages/client/ui-primitives/src/Modal.tsx`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0299-shared-modal-product-onboarding.md`.
