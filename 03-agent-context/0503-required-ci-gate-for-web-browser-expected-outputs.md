---
id: "dsh-note-0503"
title: "Required CI gate for web browser expected outputs"
status: "implemented"
class: "testing"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/testing/2026-07-30-web-browser-snapshot-ci-gate.md"
implementation_evidence: "high"
target_anchor: "repository tests and engineering policy"
tags:
  - "class/testing"
  - "concern/boundary"
  - "concern/cancellation-timeout"
  - "concern/compatibility"
  - "concern/evidence"
  - "concern/ownership"
  - "concern/performance"
  - "concern/recovery"
  - "domain/build-release"
  - "domain/extensions"
  - "domain/filesystem"
  - "domain/jobs-tasks"
  - "domain/session-state"
  - "domain/storage"
  - "domain/testing"
  - "domain/web-retrieval"
  - "lifecycle/implemented"
  - "mechanism/event-sourcing"
  - "mechanism/policy"
  - "mechanism/streaming"
aliases:
  - "record"
  - "refresh"
  - "apps/web/tests/snapshots/**/*.expected.md"
  - "DSH_SNAPSHOT=refresh"
  - "node 24 / snapshots and artifacts"
  - "scripts/run-gates.ts"
  - "ci-consumers"
  - "DSH_SNAPSHOT=replay"
  - "apps/web/dist"
  - "lib/"
  - "DSH_SNAPSHOT=refresh pnpm run test:web"
  - "web-snapshot"
  - "built-package-invariants"
  - "Required CI gate for web browser expected outputs"
search_regex: "(?i)(record|refresh|apps/web/tests/snapshots/\\*\\*/\\*\\.expected\\.md|DSH_SNAPSHOT=refresh|node[- ]24[- ]/[- ]snapshots[- ]and[- ]artifacts|scripts/run\\-gates\\.ts|ci\\-consumers|DSH_SNAPSHOT=replay)"
---

# 0503. Required CI gate for web browser expected outputs — implementation context

## Open this when

The keyless web browser e2e lane runs only under the local pnpm run test:web command, and PR CI does not compare apps/web/tests/snapshots//.expected.md. A PR that changes user-visible web output can therefore remain green when its expected outputs are not refreshed; when any later branch explicitly runs DSH_SNAPSHOT=refresh, it backfills the earlier change and produces a diff unrelated to that branch. Ordinary local runs already default to read-only replay, so the gap is mandatory enforcement at the PR level, not a ban on writes in refresh mode.

## Source decision

For Linux PRs, the node 24 / snapshots and artifacts job must run the full web browser replay/compare suite. scripts/run-gates.ts registers test:web:built as a ci-consumers gate and explicitly injects DSH_SNAPSHOT=replay; CI never runs in record or refresh mode, so when the committed goldens disagree with the currently assembled application, the tests fail directly instead of silently rewriting them on the runner and then passing. The consumer job owns the single Linux build, so apps/web/dist and the package lib/ directories remain in its workspace for the browser suite.

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/testing/2026-07-30-web-browser-snapshot-ci-gate.md](../02-notes/implemented/testing/2026-07-30-web-browser-snapshot-ci-gate.md)
- Pinned source: [.agents/notes/implemented/testing/2026-07-30-web-browser-snapshot-ci-gate.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/testing/2026-07-30-web-browser-snapshot-ci-gate.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`scripts/run-gates.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/run-gates.ts) | repository automation | The source note names this file directly. | `named-file` |
| [`packages/acp/acp/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/acp/acp/src/index.ts) | package entry point | Defines `record`, a construct named by the note. | `symbol-definition` |
| [`packages/client/ui-agent-preset/src/client/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-agent-preset/src/client/index.ts) | package entry point | Defines `refresh`, a construct named by the note. | `symbol-definition` |
| [`packages/client/ui-settings/src/client/settings-scope.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-settings/src/client/settings-scope.ts) | runtime implementation | Defines `refresh`, a construct named by the note. | `symbol-definition` |
| [`packages/client/ui-model-selection/src/client/service.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-model-selection/src/client/service.ts) | runtime implementation | Defines `refresh`, a construct named by the note. | `symbol-definition` |
| [`packages/client/ui-permission-presets/src/client/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-permission-presets/src/client/index.ts) | package entry point | Defines `refresh`, a construct named by the note. | `symbol-definition` |
| [`CLAUDE.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/CLAUDE.md) | package contract and examples | Contains the exact code literal `scripts/run-gates.ts` named by the note. | `exact-code-occurrence` |
| [`AGENTS.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/AGENTS.md) | package contract and examples | Contains the exact code literal `scripts/run-gates.ts` named by the note. | `exact-code-occurrence` |
| [`package.json`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/package.json) | composition and configuration | Contains the exact code literal `scripts/run-gates.ts` named by the note. | `exact-code-occurrence` |
| [`docs/development.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/development.md) | package contract and examples | Contains the exact code literal `scripts/run-gates.ts` named by the note. | `exact-code-occurrence` |
| [`docs/development.zh.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/development.zh.md) | package contract and examples | Contains the exact code literal `scripts/run-gates.ts` named by the note. | `exact-code-occurrence` |
| [`scripts/snapshots/translation-prompt-v4/request-response.expected.json`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/snapshots/translation-prompt-v4/request-response.expected.json) | repository automation | Contains the exact code literal `scripts/run-gates.ts` named by the note. | `exact-code-occurrence` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `record` | `const` | [`packages/acp/acp/src/index.ts:116`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/acp/acp/src/index.ts#L116) | `const record = sessions.get(agent.session.id)` |
| `record` | `const` | [`packages/acp/acp/src/index.ts:125`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/acp/acp/src/index.ts#L125) | `const record = sessions.get(sessionId)` |
| `record` | `const` | [`packages/acp/acp/src/index.ts:156`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/acp/acp/src/index.ts#L156) | `const record = sessions.get(session.header.id)` |
| `record` | `const` | [`packages/acp/acp/src/index.ts:199`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/acp/acp/src/index.ts#L199) | `const record = ownedRecord(agent)` |
| `record` | `const` | [`packages/acp/acp/src/index.ts:205`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/acp/acp/src/index.ts#L205) | `const record = ownedRecord(agent)` |
| `refresh` | `const` | [`packages/client/ui-agent-preset/src/client/index.ts:77`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-agent-preset/src/client/index.ts#L77) | `const refresh = (): void => {` |
| `refresh` | `const` | [`packages/client/ui-model-selection/src/client/service.ts:54`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-model-selection/src/client/service.ts#L54) | `const refresh = (): void => {` |
| `refresh` | `const` | [`packages/client/ui-permission-presets/src/client/index.ts:126`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-permission-presets/src/client/index.ts#L126) | `const refresh = (): void => { refreshPermissionIfLoaded(controller) }` |
| `refresh` | `const` | [`packages/client/ui-settings/src/client/settings-scope.ts:254`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-settings/src/client/settings-scope.ts#L254) | `const refresh = (namespace?: string): void => {` |

### Tests and executable evidence

- [`apps/web/tests/snapshots`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/snapshots) — The source note names this implementation area directly.
- [`scripts/run-gates.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/run-gates.spec.ts) — A test under the owning area exercises or imports `ci-consumers`. A test under the owning area exercises or imports `web-snapshot`.
- [`scripts/ci-workflow.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/ci-workflow.spec.ts) — A test under the owning area exercises or imports `apt`.
- [`apps/cli/tests/web-agent-presets.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/cli/tests/web-agent-presets.e2e.ts) — A test under the owning area exercises or imports `apt`.
- [`examples/jsonrpc-agent/tests/sdk.snapshot.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/examples/jsonrpc-agent/tests/sdk.snapshot.ts) — A test under the owning area exercises or imports `apt`.

## How to read the implementation

1. Start with [`scripts/run-gates.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/run-gates.ts) because it has the strongest evidence link to the note.
2. Read public types and entry points before implementation details. Write down the contract in plain language.
3. Follow the named constructs into their callers and tests. Confirm which component can actually enforce the rule.
4. Reconstruct one failure path: cancellation, crash, retry, stale state, partial write, or incompatible provider input—whichever the note is protecting against.
5. Only then map the contract into the current Rust repository. Target paths must come from that repository, never from guesswork in this wiki.

## Rust harness port

**Likely integration point:** repository tests and engineering policy.

Preserve the behavior and ownership rule. Do not mechanically translate Cordis, TypeScript declaration merging, Node streams, or package topology into Rust.

Relevant standing rules:

- **`rust/append-log`** — Keep durable facts append-only and derive views from them. Separate the synchronous fact boundary from asynchronous persistence.
- **`rust/capability-security`** — Make granted capabilities explicit. On the Windows no-admin path, probe support at runtime and fail closed rather than silently running unsandboxed.
- **`rust/four-tool-surface`** — Preserve the four model tools: read, write, patch, and exec. Reuse internal capabilities without automatically exposing new model-facing tools.
- **`rust/process-discipline`** — Treat this as repository or release policy unless it changes a runtime contract. Do not add runtime machinery for a process-only lesson.
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

- Tags: `class/testing`, `concern/boundary`, `concern/cancellation-timeout`, `concern/compatibility`, `concern/evidence`, `concern/ownership`, `concern/performance`, `concern/recovery`, `domain/build-release`, `domain/extensions`, `domain/filesystem`, `domain/jobs-tasks`, `domain/session-state`, `domain/storage`, `domain/testing`, `domain/web-retrieval`, `lifecycle/implemented`, `mechanism/event-sourcing`, `mechanism/policy`, `mechanism/streaming`
- Aliases: `record`, `refresh`, `apps/web/tests/snapshots/**/*.expected.md`, `DSH_SNAPSHOT=refresh`, `node 24 / snapshots and artifacts`, `scripts/run-gates.ts`, `ci-consumers`, `DSH_SNAPSHOT=replay`, `apps/web/dist`, `lib/`, `DSH_SNAPSHOT=refresh pnpm run test:web`, `web-snapshot`, `built-package-invariants`, `Required CI gate for web browser expected outputs`
- Regex: `(?i)(record|refresh|apps/web/tests/snapshots/\*\*/\*\.expected\.md|DSH_SNAPSHOT=refresh|node[- ]24[- ]/[- ]snapshots[- ]and[- ]artifacts|scripts/run\-gates\.ts|ci\-consumers|DSH_SNAPSHOT=replay)`

```bash
rg -n --pcre2 "(?i)(record|refresh|apps/web/tests/snapshots/\\*\\*/\\*\\.expected\\.md|DSH_SNAPSHOT=refresh|node[- ]24[- ]/[- ]snapshots[- ]and[- ]artifacts|scripts/run\\-gates\\.ts|ci\\-consumers|DSH_SNAPSHOT=replay)" source-deepseek-harness
rg -l --fixed-strings "class/testing" 03-agent-context
```

## Connected notes

- **`source-link`** — [0420. Independent CI consumer build](0420-independent-ci-consumer-build.md): The source note links to this decision directly.
- **`source-link`** — [0500. Keyless browser e2e lane for the web GUI](0500-keyless-browser-e2e-lane-for-the-web-gui.md): The source note links to this decision directly.
- **`shares-code-with`** — [0400. Evidence-based larger hosted runners](0400-evidence-based-larger-hosted-runners.md): Shares source implementation: `scripts/ci-workflow.spec.ts`, `scripts/run-gates.spec.ts`.
- **`shares-code-with`** — [0301. A config hot-reload must not kill or degrade a live app](0301-a-config-hot-reload-must-not-kill-or-degrade-a-live-app.md): Shares source implementation: `packages/client/ui-agent-preset/src/client/index.ts`, `packages/client/ui-model-selection/src/client/service.ts`.
- **`shares-code-with`** — [0431. Dual Wine and native Windows pull-request CI](0431-dual-wine-and-native-windows-pull-request-ci.md): Shares source implementation: `scripts/ci-workflow.spec.ts`, `scripts/run-gates.spec.ts`.
- **`shares-code-with`** — [0637. Parallel GitHub CI gates](0637-parallel-github-ci-gates.md): Shares source implementation: `scripts/run-gates.spec.ts`, `scripts/run-gates.ts`.
- **`shares-code-with`** — [0407. CI failover runbook --- hosted pools → in-house pool](0407-ci-failover-runbook-hosted-pools-in-house-pool.md): Shares source implementation: `scripts/ci-workflow.spec.ts`, `scripts/run-gates.spec.ts`.
- **`shares-code-with`** — [0404. Portable pull-request CI recovery boundary](0404-portable-pull-request-ci-recovery-boundary.md): Shares source implementation: `scripts/ci-workflow.spec.ts`, `scripts/run-gates.spec.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0503-required-ci-gate-for-web-browser-expected-outputs.md`.
