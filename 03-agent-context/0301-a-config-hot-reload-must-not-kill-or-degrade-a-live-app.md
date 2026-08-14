---
id: "dsh-note-0301"
title: "A config hot-reload must not kill or degrade a live app"
status: "implemented"
class: "bug-fix"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/bug-fix/2026-07-20-config-hot-reload-resilience.md"
implementation_evidence: "high"
target_anchor: "read/write/patch filesystem capability"
tags:
  - "class/bug-fix"
  - "concern/boundary"
  - "concern/concurrency"
  - "concern/evidence"
  - "concern/lifecycle"
  - "concern/recovery"
  - "concern/schema-types"
  - "domain/build-release"
  - "domain/configuration"
  - "domain/extensions"
  - "domain/filesystem"
  - "domain/llm"
  - "domain/observability"
  - "domain/protocols"
  - "domain/shell-terminal"
  - "domain/storage"
  - "domain/testing"
  - "lifecycle/implemented"
aliases:
  - "refresh"
  - "cordis.yml"
  - "Fiber.update"
  - "internal/update"
  - "EntryTree.await"
  - "AggregateError"
  - "registerConfig"
  - "hmr/config-update-failed"
  - "Include.refresh"
  - "packages/boot/app-boot/tests/config-reload.spec.ts"
  - "packages/boot/app-boot/tests/hmr-config.spec.ts"
  - "packages/host/webserver/tests/webserver.spec.ts"
  - "packages/typert/loader/tests/loader.spec.ts"
  - "pty-tools"
search_regex: "(?i)(refresh|cordis\\.yml|Fiber\\.update|internal/update|EntryTree\\.await|AggregateError|registerConfig|hmr/config\\-update\\-failed)"
---

# 0301. A config hot-reload must not kill or degrade a live app — implementation context

## Open this when

An invalid cordis.yml edit must not kill a running agent, but preserving the process is insufficient when a valid-looking update partially replaces the Loader tree before a later entry fails. Callers also need to observe a rejected live update without treating the same error as an unhandled boot failure. Personal configuration adds a second requirement: HMR must observe one exact file outside its module roots, including a file or parent directory created after startup.

## Source decision

The vendored Cordis lifecycle and Loader plugins provide an awaited, compensating config transaction, logged as local modifications 6, 8, and 9 in vendor/README.md. Fiber.update() returns its internal/update waterfall result. Config validation remains synchronous, while the default continuation returns the restart promise. Loader entry updates can therefore distinguish validation, import, application, and rollback failure from successful lifecycle settlement.

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/bug-fix/2026-07-20-config-hot-reload-resilience.md](../02-notes/implemented/bug-fix/2026-07-20-config-hot-reload-resilience.md)
- Pinned source: [.agents/notes/implemented/bug-fix/2026-07-20-config-hot-reload-resilience.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/bug-fix/2026-07-20-config-hot-reload-resilience.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`vendor/README.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/README.md) | package contract and examples | The source note names this file directly. Contains the exact code literal `internal/update` named by the note. | `exact-code-occurrence, named-file` |
| [`packages/client/ui-agent-preset/src/client/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-agent-preset/src/client/index.ts) | package entry point | Defines `refresh`, a construct named by the note. | `symbol-definition` |
| [`packages/client/ui-settings/src/client/settings-scope.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-settings/src/client/settings-scope.ts) | runtime implementation | Defines `refresh`, a construct named by the note. | `symbol-definition` |
| [`packages/client/ui-model-selection/src/client/service.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-model-selection/src/client/service.ts) | runtime implementation | Defines `refresh`, a construct named by the note. | `symbol-definition` |
| [`packages/client/ui-permission-presets/src/client/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-permission-presets/src/client/index.ts) | package entry point | Defines `refresh`, a construct named by the note. | `symbol-definition` |
| [`docs/cordis-api/fiber.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/cordis-api/fiber.md) | package contract and examples | Contains the exact code literal `internal/update` named by the note. | `exact-code-occurrence` |
| [`vendor/cordis/src/fiber.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/cordis/src/fiber.ts) | runtime implementation | Contains the exact code literal `internal/update` named by the note. | `exact-code-occurrence` |
| [`docs/cordis-api/fiber.zh.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/cordis-api/fiber.zh.md) | package contract and examples | Contains the exact code literal `internal/update` named by the note. | `exact-code-occurrence` |
| [`vendor/cordis/src/events.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/cordis/src/events.ts) | runtime implementation | Contains the exact code literal `internal/update` named by the note. | `exact-code-occurrence` |
| [`vendor/include/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/include/src/index.ts) | package entry point | Contains the exact code literal `internal/update` named by the note. | `exact-code-occurrence` |
| [`docs/cordis-api/inherited.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/cordis-api/inherited.md) | package contract and examples | Contains the exact code literal `internal/update` named by the note. | `exact-code-occurrence` |
| [`scripts/gen-cordis-catalog.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/gen-cordis-catalog.ts) | repository automation | Contains the exact code literal `internal/update` named by the note. | `exact-code-occurrence` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `refresh` | `const` | [`packages/client/ui-agent-preset/src/client/index.ts:77`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-agent-preset/src/client/index.ts#L77) | `const refresh = (): void => {` |
| `refresh` | `const` | [`packages/client/ui-model-selection/src/client/service.ts:54`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-model-selection/src/client/service.ts#L54) | `const refresh = (): void => {` |
| `refresh` | `const` | [`packages/client/ui-permission-presets/src/client/index.ts:126`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-permission-presets/src/client/index.ts#L126) | `const refresh = (): void => { refreshPermissionIfLoaded(controller) }` |
| `refresh` | `const` | [`packages/client/ui-settings/src/client/settings-scope.ts:254`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-settings/src/client/settings-scope.ts#L254) | `const refresh = (namespace?: string): void => {` |

### Tests and executable evidence

- [`packages/typert/loader/tests/loader.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/typert/loader/tests/loader.spec.ts) — The source note names this file directly. A test under the owning area exercises or imports `AggregateError`.
- [`packages/boot/app-boot/tests/hmr-config.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/boot/app-boot/tests/hmr-config.spec.ts) — The source note names this file directly. A test under the owning area exercises or imports `registerConfig`.
- [`packages/host/webserver/tests/webserver.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/webserver/tests/webserver.spec.ts) — The source note names this file directly.
- [`packages/boot/app-boot/tests/config-reload.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/boot/app-boot/tests/config-reload.spec.ts) — The source note names this file directly.
- [`scripts/dev-web.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/dev-web.spec.ts) — A test under the owning area exercises or imports `yml`.
- [`apps/web/tests/support.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/support.ts) — A test under the owning area exercises or imports `initial`.
- [`apps/web/tests/scaffold.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/scaffold.ts) — A test under the owning area exercises or imports `AggregateError`. A test under the owning area exercises or imports `initial`.
- [`scripts/ci-workflow.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/ci-workflow.spec.ts) — A test under the owning area exercises or imports `yml`.
- Source verification intent: packages/boot/app-boot/tests/config-reload.spec.ts boots real temporary Loader/Include trees and covers parse and shape rejection, import-before-dispose, plugin/config restoration, multi-entry rollback, ancestor disablement, overlay convergence, option identity, failed direct-update persistence, and failed programmatic moves. packages/boot/app-boot/tests/hmr-config.spec.ts covers existing and missing exact paths, add/change/removal, serialized coalescing, disposal drainage, non-Error normalization, failure broadcast, and rejecting-observer containment.

## How to read the implementation

1. Start with [`vendor/README.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/README.md) because it has the strongest evidence link to the note.
2. Read public types and entry points before implementation details. Write down the contract in plain language.
3. Follow the named constructs into their callers and tests. Confirm which component can actually enforce the rule.
4. Reconstruct one failure path: cancellation, crash, retry, stale state, partial write, or incompatible provider input—whichever the note is protecting against.
5. Only then map the contract into the current Rust repository. Target paths must come from that repository, never from guesswork in this wiki.

## Rust harness port

**Likely integration point:** read/write/patch filesystem capability.

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

- Tags: `class/bug-fix`, `concern/boundary`, `concern/concurrency`, `concern/evidence`, `concern/lifecycle`, `concern/recovery`, `concern/schema-types`, `domain/build-release`, `domain/configuration`, `domain/extensions`, `domain/filesystem`, `domain/llm`, `domain/observability`, `domain/protocols`, `domain/shell-terminal`, `domain/storage`, `domain/testing`, `lifecycle/implemented`
- Aliases: `refresh`, `cordis.yml`, `Fiber.update`, `internal/update`, `EntryTree.await`, `AggregateError`, `registerConfig`, `hmr/config-update-failed`, `Include.refresh`, `packages/boot/app-boot/tests/config-reload.spec.ts`, `packages/boot/app-boot/tests/hmr-config.spec.ts`, `packages/host/webserver/tests/webserver.spec.ts`, `packages/typert/loader/tests/loader.spec.ts`, `pty-tools`
- Regex: `(?i)(refresh|cordis\.yml|Fiber\.update|internal/update|EntryTree\.await|AggregateError|registerConfig|hmr/config\-update\-failed)`

```bash
rg -n --pcre2 "(?i)(refresh|cordis\\.yml|Fiber\\.update|internal/update|EntryTree\\.await|AggregateError|registerConfig|hmr/config\\-update\\-failed)" source-deepseek-harness
rg -l --fixed-strings "class/bug-fix" 03-agent-context
```

## Connected notes

- **`shares-code-with`** — [0503. Required CI gate for web browser expected outputs](0503-required-ci-gate-for-web-browser-expected-outputs.md): Shares source implementation: `packages/client/ui-agent-preset/src/client/index.ts`, `packages/client/ui-model-selection/src/client/service.ts`.
- **`shares-code-with`** — [0339. HMR's initial scan deadlocked a failing boot into a silent exit 13](0339-hmr-s-initial-scan-deadlocked-a-failing-boot-into-a-silent-exit-13.md): Shares source implementation: `packages/boot/app-boot/tests/config-reload.spec.ts`, `packages/boot/app-boot/tests/hmr-config.spec.ts`.
- **`shares-code-with`** — [0467. One shared base config with per-surface overlays](0467-one-shared-base-config-with-per-surface-overlays.md): Shares source implementation: `packages/boot/app-boot/tests/config-reload.spec.ts`, `vendor/README.md`.
- **`shares-code-with`** — [0439. Private npm publication as three independent sequences](0439-private-npm-publication-as-three-independent-sequences.md): Shares source implementation: `vendor/README.md`.
- **`shares-code-with`** — [0407. CI failover runbook --- hosted pools → in-house pool](0407-ci-failover-runbook-hosted-pools-in-house-pool.md): Shares source implementation: `scripts/dev-web.spec.ts`.
- **`shares-code-with`** — [0415. Make Lefthook installation worktree-local](0415-make-lefthook-installation-worktree-local.md): Shares source implementation: `scripts/dev-web.spec.ts`.
- **`same-design-pressure`** — [0431. Dual Wine and native Windows pull-request CI](0431-dual-wine-and-native-windows-pull-request-ci.md): Shares design concerns: `concern/boundary`, `concern/concurrency`, `concern/evidence`.
- **`same-design-pressure`** — [0146. The subprocess sandbox --- confinement seam, native runners, escalation, and per-session modes](0146-the-subprocess-sandbox-confinement-seam-native-runners-escalation-and-pe.md): Shares design concerns: `concern/boundary`, `concern/concurrency`, `concern/evidence`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0301-a-config-hot-reload-must-not-kill-or-degrade-a-live-app.md`.
