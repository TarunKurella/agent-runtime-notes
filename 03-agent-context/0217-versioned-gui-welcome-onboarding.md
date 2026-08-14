---
id: "dsh-note-0217"
title: "Versioned GUI welcome onboarding"
status: "implemented"
class: "feature"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/feature/2026-07-30-versioned-gui-welcome-onboarding.md"
implementation_evidence: "medium"
target_anchor: "sandbox capability boundary"
tags:
  - "class/feature"
  - "concern/boundary"
  - "concern/compatibility"
  - "concern/concurrency"
  - "concern/evidence"
  - "concern/lifecycle"
  - "concern/ownership"
  - "concern/recovery"
  - "concern/simplification"
  - "domain/configuration"
  - "domain/extensions"
  - "domain/llm"
  - "domain/observability"
  - "domain/protocols"
  - "domain/security"
  - "domain/shell-terminal"
  - "domain/storage"
  - "domain/testing"
  - "domain/ui-interaction"
  - "domain/web-retrieval"
  - "lifecycle/implemented"
  - "mechanism/streaming"
aliases:
  - "openSection"
  - "OnboardingModal"
  - "complete"
  - "settings.onboarding"
  - "ui-settings"
  - "ui-settings-models"
  - "-100"
  - "ui-settings-general"
  - "ui-onboarding"
  - "$DSH_HOME/settings.yaml"
  - "welcomeNoticeVersion"
  - "ctx.connection.isLoopback"
  - "isLoopback"
  - "$DSH_HOME"
search_regex: "(?i)(openSection|OnboardingModal|complete|settings\\.onboarding|ui\\-settings|ui\\-settings\\-models|\\-100|ui\\-settings\\-general)"
---

# 0217. Versioned GUI welcome onboarding — implementation context

## Open this when

The GUI's credential onboarding begins with a DeepSeek-specific readiness check, but the internal-test notice applies to every user and must precede provider setup even when a credential is already configured. Treating both as independent overlays permits simultaneous dialogs, while a process-local dismissal cannot distinguish a completed notice from a window closed before acknowledgement or intentionally present revised copy once.

## Source decision

The Settings shell coordinates ordered steps. settings.onboarding remains a root-scoped list, but ui-settings projects its entry ids and order into one coordinator and mounts only the first incomplete step. The active registrant receives complete() and openSection(id); no later step mounts until ownership transfers. ui-settings-models now registers the restored welcome notice at order -100 and the conditional DeepSeek credential step at order 0; their current shared presentation is owned by the shared-modal onboarding decision. The product welcome step is versioned and feature-owned.

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/feature/2026-07-30-versioned-gui-welcome-onboarding.md](../02-notes/implemented/feature/2026-07-30-versioned-gui-welcome-onboarding.md)
- Pinned source: [.agents/notes/implemented/feature/2026-07-30-versioned-gui-welcome-onboarding.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/feature/2026-07-30-versioned-gui-welcome-onboarding.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`packages/client/ui-settings/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-settings/src/index.ts) | package entry point | Core file in the package named by the note: `packages/client/ui-settings`. | `named-package-member` |
| [`packages/client/ui-settings/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-settings/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/client/ui-settings`. | `named-package-member` |
| [`packages/client/ui-settings-models/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-settings-models/src/index.ts) | package entry point | Core file in the package named by the note: `packages/client/ui-settings-models`. | `named-package-member` |
| [`packages/client/ui-settings-general/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-settings-general/src/index.ts) | package entry point | Core file in the package named by the note: `packages/client/ui-settings-general`. | `named-package-member` |
| [`packages/client/ui-settings-models/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-settings-models/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/client/ui-settings-models`. | `named-package-member` |
| [`packages/client/ui-settings-general/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-settings-general/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/client/ui-settings-general`. | `named-package-member` |
| [`packages/client/ui-settings-general/src/client/SettingsRoot.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-settings-general/src/client/SettingsRoot.tsx) | runtime implementation | Core file in the package named by the note: `packages/client/ui-settings-general`. Defines `openSection`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/client/ui-settings-models/src/client/OnboardingModal.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-settings-models/src/client/OnboardingModal.tsx) | runtime implementation | Core file in the package named by the note: `packages/client/ui-settings-models`. Defines `OnboardingModal`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/client/ui-settings`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-settings) | package or module directory | The note names this package or capability. | `named-package` |
| [`packages/client/ui-settings-models`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-settings-models) | package or module directory | The note names this package or capability. | `named-package` |
| [`packages/client/ui-settings-general`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-settings-general) | package or module directory | The note names this package or capability. | `named-package` |
| [`packages/jobs/tool-jobs/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/jobs/tool-jobs/src/index.ts) | package entry point | Defines `complete`, a construct named by the note. | `symbol-definition` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `openSection` | `const` | [`packages/client/ui-settings-general/src/client/SettingsRoot.tsx:113`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-settings-general/src/client/SettingsRoot.tsx#L113) | `const openSection = useCallback((id: string) => {` |
| `OnboardingModal` | `function` | [`packages/client/ui-settings-models/src/client/OnboardingModal.tsx:17`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-settings-models/src/client/OnboardingModal.tsx#L17) | `export function OnboardingModal({` |
| `complete` | `const` | [`packages/jobs/tool-jobs/src/index.ts:129`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/jobs/tool-jobs/src/index.ts#L129) | `const complete = \`${content}${suffix}\`` |

### Tests and executable evidence

- [`packages/client/ui-settings-models/tests/apply.client.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-settings-models/tests/apply.client.spec.ts) — A test under the owning area exercises or imports `onboarding`. A test under the owning area exercises or imports `ui-settings-models`.
- [`packages/client/ui-settings-general/tests/host.client.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-settings-general/tests/host.client.spec.ts) — A test under the owning area exercises or imports `onboarding`. A test under the owning area exercises or imports `ui-settings-general`.
- [`packages/client/ui-settings-general/tests/apply.client.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-settings-general/tests/apply.client.spec.ts) — A test under the owning area exercises or imports `onboarding`. A test under the owning area exercises or imports `ui-settings`.
- [`packages/client/ui-settings-general/tests/shell.client.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-settings-general/tests/shell.client.spec.ts) — A test under the owning area exercises or imports `onboarding`. A test under the owning area exercises or imports `isLoopback`.
- [`packages/client/ui-settings/tests/settings-scope.client.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-settings/tests/settings-scope.client.spec.ts) — A test under the owning area exercises or imports `isLoopback`.
- [`packages/client/ui-settings-models/tests/readiness.client.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-settings-models/tests/readiness.client.spec.ts) — A test under the owning area exercises or imports `onboarding`.
- [`packages/client/ui-settings-models/tests/components.client.spec.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-settings-models/tests/components.client.spec.tsx) — A test under the owning area exercises or imports `onboarding`.
- [`packages/client/ui-settings-general/tests/settings-root.client.spec.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-settings-general/tests/settings-root.client.spec.tsx) — A test under the owning area exercises or imports `onboarding`. A test under the owning area exercises or imports `openSection`.

## How to read the implementation

1. Start with [`packages/client/ui-settings/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-settings/src/index.ts) because it has the strongest evidence link to the note.
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

- Tags: `class/feature`, `concern/boundary`, `concern/compatibility`, `concern/concurrency`, `concern/evidence`, `concern/lifecycle`, `concern/ownership`, `concern/recovery`, `concern/simplification`, `domain/configuration`, `domain/extensions`, `domain/llm`, `domain/observability`, `domain/protocols`, `domain/security`, `domain/shell-terminal`, `domain/storage`, `domain/testing`, `domain/ui-interaction`, `domain/web-retrieval`, `lifecycle/implemented`, `mechanism/streaming`
- Aliases: `openSection`, `OnboardingModal`, `complete`, `settings.onboarding`, `ui-settings`, `ui-settings-models`, `-100`, `ui-settings-general`, `ui-onboarding`, `$DSH_HOME/settings.yaml`, `welcomeNoticeVersion`, `ctx.connection.isLoopback`, `isLoopback`, `$DSH_HOME`
- Regex: `(?i)(openSection|OnboardingModal|complete|settings\.onboarding|ui\-settings|ui\-settings\-models|\-100|ui\-settings\-general)`

```bash
rg -n --pcre2 "(?i)(openSection|OnboardingModal|complete|settings\\.onboarding|ui\\-settings|ui\\-settings\\-models|\\-100|ui\\-settings\\-general)" source-deepseek-harness
rg -l --fixed-strings "class/feature" 03-agent-context
```

## Connected notes

- **`source-link`** — [0299. Shared-modal product onboarding](0299-shared-modal-product-onboarding.md): The source note links to this decision directly.
- **`source-link`** — [0493. Remove the first-run beta notice](0493-remove-the-first-run-beta-notice.md): The source note links to this decision directly.
- **`shares-code-with`** — [0349. onboarding takeover chrome moves into the step](0349-onboarding-takeover-chrome-moves-into-the-step.md): Shares source implementation: `packages/client/ui-settings-general/src/client/SettingsRoot.tsx`, `packages/client/ui-settings-general/src/index.ts`.
- **`shares-code-with`** — [0213. official DeepSeek first-run credential setup](0213-official-deepseek-first-run-credential-setup.md): Shares source implementation: `packages/client/ui-settings`, `packages/client/ui-settings-models/src/index.ts`.
- **`shares-code-with`** — [0125. Feature-owned tabs in Plugins settings](0125-feature-owned-tabs-in-plugins-settings.md): Shares source implementation: `packages/client/ui-settings`, `packages/client/ui-settings-general/src/index.ts`.
- **`same-design-pressure`** — [0500. Keyless browser e2e lane for the web GUI](0500-keyless-browser-e2e-lane-for-the-web-gui.md): Shares design concerns: `concern/boundary`, `concern/compatibility`, `concern/concurrency`.
- **`same-design-pressure`** — [0542. Dependency swaps rejected by the 2026-07 NIH audit](0542-dependency-swaps-rejected-by-the-2026-07-nih-audit.md): Shares design concerns: `concern/boundary`, `concern/compatibility`, `concern/concurrency`.
- **`same-design-pressure`** — [0146. The subprocess sandbox --- confinement seam, native runners, escalation, and per-session modes](0146-the-subprocess-sandbox-confinement-seam-native-runners-escalation-and-pe.md): Shares design concerns: `concern/boundary`, `concern/compatibility`, `concern/concurrency`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0217-versioned-gui-welcome-onboarding.md`.
