---
id: "dsh-note-0349"
title: "onboarding takeover chrome moves into the step"
status: "implemented"
class: "bug-fix"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/bug-fix/2026-08-06-onboarding-step-owned-takeover-chrome.md"
implementation_evidence: "high"
target_anchor: "sandbox capability boundary"
tags:
  - "class/bug-fix"
  - "concern/boundary"
  - "concern/concurrency"
  - "concern/discovery-routing"
  - "concern/evidence"
  - "concern/ownership"
  - "domain/configuration"
  - "domain/extensions"
  - "domain/filesystem"
  - "domain/security"
  - "domain/session-state"
  - "domain/shell-terminal"
  - "domain/testing"
  - "domain/ui-interaction"
  - "domain/web-retrieval"
  - "lifecycle/implemented"
  - "mechanism/projection"
  - "mechanism/registry"
aliases:
  - "OnboardingSurface"
  - "SettingsRoot"
  - "openSection"
  - "ready"
  - "describe"
  - "complete"
  - "--dsw-alias-bg-layer-1"
  - "settings.onboarding"
  - "SettingsRoot.module.css"
  - "stepId"
  - "renderSlot"
  - "packages/client/ui-primitives/tests/onboarding-surface.client.spec.tsx"
  - "apps/web/tests/onboarding-deepseek-config.e2e.ts"
  - "settings.describe"
search_regex: "(?i)(OnboardingSurface|SettingsRoot|openSection|ready|describe|complete|\\-\\-dsw\\-alias\\-bg\\-layer\\-1|settings\\.onboarding)"
---

# 0349. onboarding takeover chrome moves into the step — implementation context

## Open this when

The settings shell mounted the onboarding takeover chrome --- a body-portaled overlay with an opaque --dsw-alias-bg-layer-1 stage, a blur mask, and #root set inert --- the moment a settings.onboarding step was registered and not yet locally completed. Every step decides whether it actually needs to show by loading a private fact first (WelcomeNotice: the acknowledgement bit through its settings join; DeepSeekOnboardingDialog: credential readiness through the Models join) and renders null while that fact is in flight.

## Source decision

The takeover chrome belongs to the step, not the shell. A new zero-cordis primitive, OnboardingSurface (ui-primitives), renders the body-portaled overlay/mask/stage --- CSS class names and geometry moved verbatim from SettingsRoot.module.css --- and holds #root inert for exactly its own mount lifetime. Both step components wrap only their visible branch in it; their existing null branches now paint and block nothing by construction, because the chrome is part of the same render decision.

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/bug-fix/2026-08-06-onboarding-step-owned-takeover-chrome.md](../02-notes/implemented/bug-fix/2026-08-06-onboarding-step-owned-takeover-chrome.md)
- Pinned source: [.agents/notes/implemented/bug-fix/2026-08-06-onboarding-step-owned-takeover-chrome.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/bug-fix/2026-08-06-onboarding-step-owned-takeover-chrome.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`packages/client/ui-settings-models/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-settings-models/src/index.ts) | package entry point | Core file in the package named by the note: `packages/client/ui-settings-models`. | `named-package-member` |
| [`packages/client/ui-settings-general/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-settings-general/src/index.ts) | package entry point | Core file in the package named by the note: `packages/client/ui-settings-general`. | `named-package-member` |
| [`packages/client/ui-settings-models/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-settings-models/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/client/ui-settings-models`. | `named-package-member` |
| [`packages/client/ui-settings-general/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-settings-general/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/client/ui-settings-general`. | `named-package-member` |
| [`packages/client/ui-settings-general/src/client/SettingsRoot.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-settings-general/src/client/SettingsRoot.tsx) | runtime implementation | Core file in the package named by the note: `packages/client/ui-settings-general`. Defines `SettingsRoot`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/client/ui-settings-models/src/client/CustomProviderCard.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-settings-models/src/client/CustomProviderCard.tsx) | provider/backend adapter | Core file in the package named by the note: `packages/client/ui-settings-models`. Defines `ready`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/client/ui-settings-models`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-settings-models) | package or module directory | The note names this package or capability. | `named-package` |
| [`packages/client/ui-settings-general`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-settings-general) | package or module directory | The note names this package or capability. | `named-package` |
| [`packages/core/tools/src/py-types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/py-types.ts) | runtime implementation | Defines `describe`, a construct named by the note. | `symbol-definition` |
| [`packages/jobs/tool-jobs/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/jobs/tool-jobs/src/index.ts) | package entry point | Defines `complete`, a construct named by the note. | `symbol-definition` |
| [`packages/client/ui-primitives/src/OnboardingSurface.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-primitives/src/OnboardingSurface.tsx) | runtime implementation | Defines `OnboardingSurface`, a construct named by the note. | `symbol-definition` |
| [`packages/client/ui-settings-models/README.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-settings-models/README.md) | package contract and examples | Core file in the package named by the note: `packages/client/ui-settings-models`. | `named-package-member` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `OnboardingSurface` | `function` | [`packages/client/ui-primitives/src/OnboardingSurface.tsx:20`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-primitives/src/OnboardingSurface.tsx#L20) | `export function OnboardingSurface({ children }: { children: ReactNode }) {` |
| `SettingsRoot` | `function` | [`packages/client/ui-settings-general/src/client/SettingsRoot.tsx:104`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-settings-general/src/client/SettingsRoot.tsx#L104) | `export function SettingsRoot(props: SettingsRootComponentProps) {` |
| `openSection` | `const` | [`packages/client/ui-settings-general/src/client/SettingsRoot.tsx:113`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-settings-general/src/client/SettingsRoot.tsx#L113) | `const openSection = useCallback((id: string) => {` |
| `ready` | `const` | [`packages/client/ui-settings-models/src/client/CustomProviderCard.tsx:110`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-settings-models/src/client/CustomProviderCard.tsx#L110) | `const ready = route.length > 0 && !routeInvalid && !routeTaken` |
| `describe` | `function` | [`packages/core/tools/src/py-types.ts:223`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/py-types.ts#L223) | `function describe(schema: object): string \| undefined {` |
| `complete` | `const` | [`packages/jobs/tool-jobs/src/index.ts:129`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/jobs/tool-jobs/src/index.ts#L129) | `const complete = \`${content}${suffix}\`` |

### Tests and executable evidence

- [`apps/web/tests/onboarding-deepseek-config.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/onboarding-deepseek-config.e2e.ts) — The source note names this file directly.
- [`packages/client/ui-primitives/tests/onboarding-surface.client.spec.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-primitives/tests/onboarding-surface.client.spec.tsx) — The source note names this file directly. A test under the owning area exercises or imports `OnboardingSurface`.
- [`packages/client/ui-settings-general/tests/settings-root.client.spec.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-settings-general/tests/settings-root.client.spec.tsx) — The source note names this file directly. A test under the owning area exercises or imports `onboarding`.
- [`packages/client/ui-settings-models/tests/apply.client.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-settings-models/tests/apply.client.spec.ts) — A test under the owning area exercises or imports `onboarding`. A test under the owning area exercises or imports `ui-settings-models`.
- [`packages/client/ui-settings-general/tests/host.client.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-settings-general/tests/host.client.spec.ts) — A test under the owning area exercises or imports `onboarding`. A test under the owning area exercises or imports `ui-settings-general`.
- [`packages/client/ui-settings-general/tests/apply.client.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-settings-general/tests/apply.client.spec.ts) — A test under the owning area exercises or imports `onboarding`. A test under the owning area exercises or imports `ui-settings-general`.
- [`packages/client/ui-settings-general/tests/shell.client.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-settings-general/tests/shell.client.spec.ts) — A test under the owning area exercises or imports `onboarding`. A test under the owning area exercises or imports `SettingsRoot`.
- [`packages/client/ui-settings-models/tests/styles.client.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-settings-models/tests/styles.client.spec.ts) — A test under the owning area exercises or imports `css`.
- Source verification intent: packages/client/ui-primitives/tests/onboarding-surface.client.spec.tsx pins the primitive: body portal around the content, mask/stage class presence, #root inert held for exactly the mount lifetime, and the no-#root composition. packages/client/ui-settings-general/tests/settings-root.client.spec.tsx pins the inverted shell contract: no takeover chrome and no inert while a mounted step renders nothing.

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

- Tags: `class/bug-fix`, `concern/boundary`, `concern/concurrency`, `concern/discovery-routing`, `concern/evidence`, `concern/ownership`, `domain/configuration`, `domain/extensions`, `domain/filesystem`, `domain/security`, `domain/session-state`, `domain/shell-terminal`, `domain/testing`, `domain/ui-interaction`, `domain/web-retrieval`, `lifecycle/implemented`, `mechanism/projection`, `mechanism/registry`
- Aliases: `OnboardingSurface`, `SettingsRoot`, `openSection`, `ready`, `describe`, `complete`, `--dsw-alias-bg-layer-1`, `settings.onboarding`, `SettingsRoot.module.css`, `stepId`, `renderSlot`, `packages/client/ui-primitives/tests/onboarding-surface.client.spec.tsx`, `apps/web/tests/onboarding-deepseek-config.e2e.ts`, `settings.describe`
- Regex: `(?i)(OnboardingSurface|SettingsRoot|openSection|ready|describe|complete|\-\-dsw\-alias\-bg\-layer\-1|settings\.onboarding)`

```bash
rg -n --pcre2 "(?i)(OnboardingSurface|SettingsRoot|openSection|ready|describe|complete|\\-\\-dsw\\-alias\\-bg\\-layer\\-1|settings\\.onboarding)" source-deepseek-harness
rg -l --fixed-strings "class/bug-fix" 03-agent-context
```

## Connected notes

- **`shares-code-with`** — [0217. Versioned GUI welcome onboarding](0217-versioned-gui-welcome-onboarding.md): Shares source implementation: `packages/client/ui-settings-general/src/client/SettingsRoot.tsx`, `packages/client/ui-settings-general/src/index.ts`.
- **`shares-code-with`** — [0493. Remove the first-run beta notice](0493-remove-the-first-run-beta-notice.md): Shares source implementation: `packages/client/ui-settings-general/src/index.ts`, `packages/client/ui-settings-general/src/invariant.ts`.
- **`shares-code-with`** — [0299. Shared-modal product onboarding](0299-shared-modal-product-onboarding.md): Shares source implementation: `packages/client/ui-settings-models`, `packages/client/ui-settings-models/src/index.ts`.
- **`shares-code-with`** — [0213. official DeepSeek first-run credential setup](0213-official-deepseek-first-run-credential-setup.md): Shares source implementation: `packages/client/ui-settings-models/src/index.ts`, `packages/client/ui-settings-models/src/invariant.ts`.
- **`shares-code-with`** — [0125. Feature-owned tabs in Plugins settings](0125-feature-owned-tabs-in-plugins-settings.md): Shares source implementation: `packages/client/ui-settings-general/src/index.ts`, `packages/client/ui-settings-general/src/invariant.ts`.
- **`shares-code-with`** — [0574. The banner sweeps in; the subtitle line is gone](0574-the-banner-sweeps-in-the-subtitle-line-is-gone.md): Shares source implementation: `apps/web/tests/onboarding-deepseek-config.e2e.ts`, `packages/client/ui-settings-general/tests/settings-root.client.spec.tsx`.
- **`shares-code-with`** — [0568. Startup slogans replace the configured TUI welcome line](0568-startup-slogans-replace-the-configured-tui-welcome-line.md): Shares source implementation: `apps/web/tests/onboarding-deepseek-config.e2e.ts`.
- **`same-design-pressure`** — [0500. Keyless browser e2e lane for the web GUI](0500-keyless-browser-e2e-lane-for-the-web-gui.md): Shares design concerns: `concern/boundary`, `concern/concurrency`, `concern/discovery-routing`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0349-onboarding-takeover-chrome-moves-into-the-step.md`.
