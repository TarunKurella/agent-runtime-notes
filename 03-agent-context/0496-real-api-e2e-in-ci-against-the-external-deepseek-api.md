---
id: "dsh-note-0496"
title: "Real-API e2e in CI against the external DeepSeek API"
status: "implemented"
class: "testing"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/testing/2026-06-19-real-api-e2e-ci.md"
implementation_evidence: "high"
target_anchor: "repository tests and engineering policy"
tags:
  - "class/testing"
  - "concern/boundary"
  - "concern/cancellation-timeout"
  - "concern/compatibility"
  - "concern/concurrency"
  - "concern/discovery-routing"
  - "concern/evidence"
  - "concern/human-control"
  - "concern/lifecycle"
  - "concern/ownership"
  - "concern/recovery"
  - "concern/trust"
  - "domain/build-release"
  - "domain/configuration"
  - "domain/filesystem"
  - "domain/jobs-tasks"
  - "domain/llm"
  - "domain/protocols"
  - "domain/security"
  - "domain/session-state"
  - "domain/shell-terminal"
  - "domain/testing"
  - "domain/ui-interaction"
  - "lifecycle/implemented"
  - "mechanism/adapter"
  - "mechanism/policy"
  - "mechanism/projection"
aliases:
  - "push"
  - "schedule"
  - "PUBLIC_BASE_URL"
  - "main"
  - "*.e2e.ts"
  - "describe.skipIf"
  - "workflow_dispatch"
  - "pull_request"
  - "head.repo.fork == false"
  - "pull_request.user.login"
  - "github.actor"
  - "DEEPSEEK_API_KEY_EXTERNAL"
  - "DEEPSEEK_API_KEY"
  - "process.env.DEEPSEEK_API_KEY"
search_regex: "(?i)(push|schedule|PUBLIC_BASE_URL|main|\\*\\.e2e\\.ts|describe\\.skipIf|workflow_dispatch|pull_request)"
---

# 0496. Real-API e2e in CI against the external DeepSeek API — implementation context

## Open this when

The harness leans hard on real-API tests by policy: docs/testing.md argues that a no-key suite proves the plumbing but not the product, and the ACP inject postmortem is the standing proof --- 178 keyless tests stayed green while a real ACP client session crashed instantly. The real-API e2e suite (pnpm run test:e2e, the .e2e.ts files) exists precisely to close that gap: it drives the agent against the live DeepSeek API --- real model calls, real bash tools, multi-turn, resume, ACP-over-stdio. The default gate (.github/workflows/ci.yml) is deliberately keyless: it carries no secret and runs for forks.

## Source decision

A dedicated workflow, .github/workflows/e2e.yml, separate from ci.yml, runs only pnpm run test:e2e against the external API using a repo secret, on trusted events, with a preflight that converts a missing secret into a loud failure instead of a false green. The keyless workflow remains separate so forkable quality gates and secret-consuming real-API gates keep different trigger and credential policies. ci.yml's value is that it is keyless, forkable, and always-green: any contributor (including an outside fork) gets a complete keyless signal with no secret in the blast radius.

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/testing/2026-06-19-real-api-e2e-ci.md](../02-notes/implemented/testing/2026-06-19-real-api-e2e-ci.md)
- Pinned source: [.agents/notes/implemented/testing/2026-06-19-real-api-e2e-ci.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/testing/2026-06-19-real-api-e2e-ci.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`docs/testing.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/testing.md) | package contract and examples | The source note names this file directly. | `named-file` |
| [`.github/workflows/ci.yml`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.github/workflows/ci.yml) | repository automation | The source note names this file directly. | `named-file` |
| [`.github/workflows/e2e.yml`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.github/workflows/e2e.yml) | repository automation | The source note names this file directly. | `named-file` |
| [`packages/llm/llm-deepseek/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm-deepseek/src/index.ts) | package entry point | The source note names this file directly. Defines `PUBLIC_BASE_URL`, a construct named by the note. | `named-file, symbol-definition` |
| [`docs/postmortem/0001-acp-default-export-drops-inject.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/postmortem/0001-acp-default-export-drops-inject.md) | package contract and examples | The source note names this file directly. | `named-file` |
| [`packages/schedule/schedule/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/schedule/schedule/src/index.ts) | package entry point | Core file in the package named by the note: `packages/schedule/schedule`. | `named-package-member` |
| [`packages/schedule/schedule/src/types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/schedule/schedule/src/types.ts) | public types and contract | Core file in the package named by the note: `packages/schedule/schedule`. | `named-package-member` |
| [`packages/schedule/schedule/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/schedule/schedule/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/schedule/schedule`. | `named-package-member` |
| [`packages/schedule/schedule`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/packages/schedule/schedule) | package or module directory | The note names this package or capability. | `named-package` |
| [`packages/client/connection/src/client/fixture.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/connection/src/client/fixture.ts) | runtime implementation | Defines `push`, a construct named by the note. | `symbol-definition` |
| [`packages/sandbox/sandbox-windows-acl/src/runner.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/sandbox/sandbox-windows-acl/src/runner.ts) | runtime implementation | Defines `main`, a construct named by the note. | `symbol-definition` |
| [`packages/client/runtime/src/client/contract/store.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/runtime/src/client/contract/store.ts) | runtime implementation | Defines `schedule`, a construct named by the note. | `symbol-definition` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `push` | `const` | [`packages/client/connection/src/client/fixture.ts:361`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/connection/src/client/fixture.ts#L361) | `const push = (e: Record<string, unknown>): number => {` |
| `schedule` | `const` | [`packages/client/runtime/src/client/contract/store.ts:58`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/runtime/src/client/contract/store.ts#L58) | `const schedule: (fn: () => void) => void =` |
| `PUBLIC_BASE_URL` | `const` | [`packages/llm/llm-deepseek/src/index.ts:104`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm-deepseek/src/index.ts#L104) | `export const PUBLIC_BASE_URL = 'https://api.deepseek.com'` |
| `main` | `function` | [`packages/sandbox/sandbox-windows-acl/src/runner.ts:115`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/sandbox/sandbox-windows-acl/src/runner.ts#L115) | `async function main(): Promise<number> {` |

### Tests and executable evidence

- No test file was tied to this note with enough confidence. Read the note's verification section and search the owning package's `tests/` directory.

## How to read the implementation

1. Start with [`docs/testing.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/testing.md) because it has the strongest evidence link to the note.
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

- Tags: `class/testing`, `concern/boundary`, `concern/cancellation-timeout`, `concern/compatibility`, `concern/concurrency`, `concern/discovery-routing`, `concern/evidence`, `concern/human-control`, `concern/lifecycle`, `concern/ownership`, `concern/recovery`, `concern/trust`, `domain/build-release`, `domain/configuration`, `domain/filesystem`, `domain/jobs-tasks`, `domain/llm`, `domain/protocols`, `domain/security`, `domain/session-state`, `domain/shell-terminal`, `domain/testing`, `domain/ui-interaction`, `lifecycle/implemented`, `mechanism/adapter`, `mechanism/policy`, `mechanism/projection`
- Aliases: `push`, `schedule`, `PUBLIC_BASE_URL`, `main`, `*.e2e.ts`, `describe.skipIf`, `workflow_dispatch`, `pull_request`, `head.repo.fork == false`, `pull_request.user.login`, `github.actor`, `DEEPSEEK_API_KEY_EXTERNAL`, `DEEPSEEK_API_KEY`, `process.env.DEEPSEEK_API_KEY`
- Regex: `(?i)(push|schedule|PUBLIC_BASE_URL|main|\*\.e2e\.ts|describe\.skipIf|workflow_dispatch|pull_request)`

```bash
rg -n --pcre2 "(?i)(push|schedule|PUBLIC_BASE_URL|main|\\*\\.e2e\\.ts|describe\\.skipIf|workflow_dispatch|pull_request)" source-deepseek-harness
rg -l --fixed-strings "class/testing" 03-agent-context
```

## Connected notes

- **`shares-code-with`** — [0481. Conversational Schedule delivery](0481-conversational-schedule-delivery.md): Shares source implementation: `packages/schedule/schedule`, `packages/schedule/schedule/src/index.ts`.
- **`shares-code-with`** — [0500. Keyless browser e2e lane for the web GUI](0500-keyless-browser-e2e-lane-for-the-web-gui.md): Shares source implementation: `docs/testing.md`, `packages/llm/llm-deepseek/src/index.ts`.
- **`shares-code-with`** — [0257. Durable Session-local reminders](0257-durable-session-local-reminders.md): Shares source implementation: `packages/schedule/schedule/src/index.ts`, `packages/schedule/schedule/src/invariant.ts`.
- **`shares-code-with`** — [0495. ACP snapshot tests --- record-once / replay-deterministic](0495-acp-snapshot-tests-record-once-replay-deterministic.md): Shares source implementation: `docs/postmortem/0001-acp-default-export-drops-inject.md`, `packages/llm/llm-deepseek/src/index.ts`.
- **`shares-code-with`** — [0407. CI failover runbook --- hosted pools → in-house pool](0407-ci-failover-runbook-hosted-pools-in-house-pool.md): Shares source implementation: `.github/workflows/ci.yml`, `packages/client/connection/src/client/fixture.ts`.
- **`shares-code-with`** — [0431. Dual Wine and native Windows pull-request CI](0431-dual-wine-and-native-windows-pull-request-ci.md): Shares source implementation: `.github/workflows/ci.yml`.
- **`shares-code-with`** — [0018. Web capability seam - stable tools over multiple providers](0018-web-capability-seam-stable-tools-over-multiple-providers.md): Shares source implementation: `docs/postmortem/0001-acp-default-export-drops-inject.md`.
- **`shares-code-with`** — [0399. Serial cross-platform CI reference](0399-serial-cross-platform-ci-reference.md): Shares source implementation: `.github/workflows/ci.yml`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0496-real-api-e2e-in-ci-against-the-external-deepseek-api.md`.
