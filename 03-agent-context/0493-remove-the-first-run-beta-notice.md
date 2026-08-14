---
id: "dsh-note-0493"
title: "Remove the first-run beta notice"
status: "implemented"
class: "simplification"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/simplification/2026-08-13-remove-first-run-beta-notice.md"
implementation_evidence: "medium"
target_anchor: "sandbox capability boundary"
tags:
  - "class/simplification"
  - "concern/boundary"
  - "concern/compatibility"
  - "concern/evidence"
  - "concern/ownership"
  - "concern/simplification"
  - "domain/build-release"
  - "domain/configuration"
  - "domain/llm"
  - "domain/observability"
  - "domain/security"
  - "domain/session-state"
  - "domain/testing"
  - "domain/ui-interaction"
  - "lifecycle/implemented"
  - "mechanism/generation"
  - "mechanism/registry"
aliases:
  - "DSH_TELEMETRY_MODE"
  - "DISABLED"
  - "ui-settings-general"
  - "settings.onboarding"
  - "ui-onboarding"
  - "ui-settings-models"
  - "welcomeNoticeVersion"
  - "Remove the first-run beta notice"
  - "simplification"
  - "boundary"
  - "compatibility"
  - "evidence"
  - "ownership"
  - "build release"
search_regex: "(?i)(DSH_TELEMETRY_MODE|DISABLED|ui\\-settings\\-general|settings\\.onboarding|ui\\-onboarding|ui\\-settings\\-models|welcomeNoticeVersion|Remove[- ]the[- ]first\\-run[- ]beta[- ]notice)"
---

# 0493. Remove the first-run beta notice — implementation context

## Open this when

Every GUI first launch opened with a full-viewport internal-test statement (内测声明): internal-beta framing plus instructions for enabling Session Log upload through DSH_TELEMETRY_MODE. Session telemetry already resolves to DISABLED when its mode is unset (telemetry default-off), so the only onboarding content about telemetry was a prompt explaining how to turn it on, and the internal-test framing itself must not ship in a release build.

## Source decision

This decision removed the first-run notice from the assembled product rather than rewording it. ui-settings-general seated no settings.onboarding step; the notice component, acknowledgement store, copy owner, and locale keys were deleted, while the Host kept the ui-onboarding namespace so stored documents remained valid. The later shared-modal product onboarding restores a new concise testing-stage notice in ui-settings-models, reusing that field and backend contract without restoring the removed takeover layout or telemetry instructions.

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/simplification/2026-08-13-remove-first-run-beta-notice.md](../02-notes/implemented/simplification/2026-08-13-remove-first-run-beta-notice.md)
- Pinned source: [.agents/notes/implemented/simplification/2026-08-13-remove-first-run-beta-notice.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/simplification/2026-08-13-remove-first-run-beta-notice.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`apps/cli/reference/README.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/cli/reference/README.md) | package contract and examples | The source note names this file directly. | `named-file` |
| [`packages/client/ui-settings-models/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-settings-models/src/index.ts) | package entry point | Core file in the package named by the note: `packages/client/ui-settings-models`. | `named-package-member` |
| [`packages/client/ui-settings-general/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-settings-general/src/index.ts) | package entry point | Core file in the package named by the note: `packages/client/ui-settings-general`. | `named-package-member` |
| [`packages/client/ui-settings-models/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-settings-models/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/client/ui-settings-models`. | `named-package-member` |
| [`packages/client/ui-settings-general/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-settings-general/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/client/ui-settings-general`. | `named-package-member` |
| [`packages/client/ui-settings-models`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-settings-models) | package or module directory | The note names this package or capability. | `named-package` |
| [`packages/client/ui-settings-general`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-settings-general) | package or module directory | The note names this package or capability. | `named-package` |
| [`packages/client/ui-settings-models/README.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-settings-models/README.md) | package contract and examples | Core file in the package named by the note: `packages/client/ui-settings-models`. | `named-package-member` |
| [`packages/client/ui-settings-general/README.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-settings-general/README.md) | package contract and examples | Core file in the package named by the note: `packages/client/ui-settings-general`. | `named-package-member` |
| [`packages/client/ui-settings-models/package.json`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-settings-models/package.json) | composition and configuration | Core file in the package named by the note: `packages/client/ui-settings-models`. | `named-package-member` |
| [`packages/client/ui-settings-general/package.json`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-settings-general/package.json) | composition and configuration | Core file in the package named by the note: `packages/client/ui-settings-general`. | `named-package-member` |

### Code constructs named by the note

The note names behavior or policy but no uniquely resolvable code declaration. Search by its aliases and inspect the listed package entry points.

### Tests and executable evidence

- [`packages/client/ui-settings-general/tests/host.client.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-settings-general/tests/host.client.spec.ts) — A test under the owning area exercises or imports `ui-settings-general`. A test under the owning area exercises or imports `onboarding`.
- [`packages/client/ui-settings-models/tests/apply.client.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-settings-models/tests/apply.client.spec.ts) — A test under the owning area exercises or imports `onboarding`. A test under the owning area exercises or imports `ui-onboarding`.
- [`packages/client/ui-settings-general/tests/apply.client.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-settings-general/tests/apply.client.spec.ts) — A test under the owning area exercises or imports `ui-settings-general`. A test under the owning area exercises or imports `onboarding`.
- [`packages/client/ui-settings-general/tests/shell.client.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-settings-general/tests/shell.client.spec.ts) — A test under the owning area exercises or imports `onboarding`.
- [`packages/client/ui-settings-models/tests/readiness.client.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-settings-models/tests/readiness.client.spec.ts) — A test under the owning area exercises or imports `onboarding`.
- [`packages/client/ui-settings-models/tests/components.client.spec.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-settings-models/tests/components.client.spec.tsx) — A test under the owning area exercises or imports `onboarding`.
- [`packages/client/ui-settings-general/tests/settings-root.client.spec.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-settings-general/tests/settings-root.client.spec.tsx) — A test under the owning area exercises or imports `onboarding`.

## How to read the implementation

1. Start with [`apps/cli/reference/README.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/cli/reference/README.md) because it has the strongest evidence link to the note.
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

- Tags: `class/simplification`, `concern/boundary`, `concern/compatibility`, `concern/evidence`, `concern/ownership`, `concern/simplification`, `domain/build-release`, `domain/configuration`, `domain/llm`, `domain/observability`, `domain/security`, `domain/session-state`, `domain/testing`, `domain/ui-interaction`, `lifecycle/implemented`, `mechanism/generation`, `mechanism/registry`
- Aliases: `DSH_TELEMETRY_MODE`, `DISABLED`, `ui-settings-general`, `settings.onboarding`, `ui-onboarding`, `ui-settings-models`, `welcomeNoticeVersion`, `Remove the first-run beta notice`, `simplification`, `boundary`, `compatibility`, `evidence`, `ownership`, `build release`
- Regex: `(?i)(DSH_TELEMETRY_MODE|DISABLED|ui\-settings\-general|settings\.onboarding|ui\-onboarding|ui\-settings\-models|welcomeNoticeVersion|Remove[- ]the[- ]first\-run[- ]beta[- ]notice)`

```bash
rg -n --pcre2 "(?i)(DSH_TELEMETRY_MODE|DISABLED|ui\\-settings\\-general|settings\\.onboarding|ui\\-onboarding|ui\\-settings\\-models|welcomeNoticeVersion|Remove[- ]the[- ]first\\-run[- ]beta[- ]notice)" source-deepseek-harness
rg -l --fixed-strings "class/simplification" 03-agent-context
```

## Connected notes

- **`source-link`** — [0286. SessionTelemetryBackend requires explicit opt-in](0286-sessiontelemetrybackend-requires-explicit-opt-in.md): The source note links to this decision directly.
- **`source-link`** — [0299. Shared-modal product onboarding](0299-shared-modal-product-onboarding.md): The source note links to this decision directly.
- **`shares-code-with`** — [0217. Versioned GUI welcome onboarding](0217-versioned-gui-welcome-onboarding.md): Shares source implementation: `packages/client/ui-settings-general/src/index.ts`, `packages/client/ui-settings-general/src/invariant.ts`.
- **`shares-code-with`** — [0349. onboarding takeover chrome moves into the step](0349-onboarding-takeover-chrome-moves-into-the-step.md): Shares source implementation: `packages/client/ui-settings-general/src/index.ts`, `packages/client/ui-settings-general/src/invariant.ts`.
- **`shares-code-with`** — [0125. Feature-owned tabs in Plugins settings](0125-feature-owned-tabs-in-plugins-settings.md): Shares source implementation: `packages/client/ui-settings-general`, `packages/client/ui-settings-general/src/index.ts`.
- **`shares-code-with`** — [0213. official DeepSeek first-run credential setup](0213-official-deepseek-first-run-credential-setup.md): Shares source implementation: `packages/client/ui-settings-models/src/index.ts`, `packages/client/ui-settings-models/src/invariant.ts`.
- **`same-design-pressure`** — [0509. Domain KV storage capability seam and the workspace entity](0509-domain-kv-storage-capability-seam-and-the-workspace-entity.md): Shares design concerns: `concern/boundary`, `concern/compatibility`, `concern/evidence`.
- **`same-design-pressure`** — [0147. MCP client plugin --- connect to external MCP servers and bridge their tools](0147-mcp-client-plugin-connect-to-external-mcp-servers-and-bridge-their-tools.md): Shares design concerns: `concern/boundary`, `concern/compatibility`, `concern/evidence`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0493-remove-the-first-run-beta-notice.md`.
