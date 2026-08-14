---
id: "dsh-note-0684"
title: "Adopt execa for hand-rolled test subprocess plumbing"
status: "archived"
class: "testing"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/archived/testing/2026-07-26-execa-for-test-subprocess-plumbing.md"
implementation_evidence: "high"
target_anchor: "repository tests and engineering policy"
tags:
  - "class/testing"
  - "concern/boundary"
  - "concern/cancellation-timeout"
  - "concern/compatibility"
  - "concern/evidence"
  - "concern/human-control"
  - "concern/lifecycle"
  - "concern/ownership"
  - "concern/recovery"
  - "concern/schema-types"
  - "concern/simplification"
  - "domain/build-release"
  - "domain/configuration"
  - "domain/llm"
  - "domain/protocols"
  - "domain/session-state"
  - "domain/shell-terminal"
  - "domain/storage"
  - "domain/testing"
  - "domain/ui-interaction"
  - "lifecycle/archived"
  - "mechanism/policy"
  - "mechanism/projection"
  - "mechanism/streaming"
aliases:
  - "parseArgs"
  - "kill"
  - "waitForPersistedTurnStart"
  - "waitForPersistedTurnEnd"
  - "waitForWorkspaceFile"
  - "runLoaderSmoke"
  - "setEncoding"
  - "setTimeout"
  - "packages/support/loader-smoke/src/index.ts"
  - "runBuiltBin"
  - "apps/cli/tests/built-bin.e2e.ts"
  - "packages/examples/cli-demo/tests/built-bin.e2e.ts"
  - "runBinExpectingExit"
  - "packages/examples/acp-demo/tests/built-bin.e2e.ts"
search_regex: "(?i)(parseArgs|kill|waitForPersistedTurnStart|waitForPersistedTurnEnd|waitForWorkspaceFile|runLoaderSmoke|setEncoding|setTimeout)"
---

# 0684. Adopt execa for hand-rolled test subprocess plumbing — implementation context

## Open this when

Roughly ten e2e/smoke files re-derived the same spawn-collect-timeout choreography by hand: let stdout = '' accumulation with setEncoding and data handlers, a setTimeout → kill('SIGKILL') deadline, and once('exit')/once('error') settlement, each with small variations. The sites: the inner spawn block of runLoaderSmoke (packages/support/loader-smoke/src/index.ts), runBuiltBin in apps/cli/tests/built-bin.e2e.ts and packages/examples/cli-demo/tests/built-bin.e2e.ts, runBinExpectingExit in packages/examples/acp-demo/tests/built-bin.e2e.ts, the built-lib e2e helpers in lsp-local and code-runtime-worker, the outer.

## Source decision

execa is a root devDependency and a runtime dependency of @deepseek-ai/dsh-loader-smoke (the one src/ consumer). The listed spawn-collect-timeout sites run through await execa(cmd, args, { cwd, env, timeout, killSignal: 'SIGKILL', reject: false }), whose result reports { stdout, stderr, exitCode, signal, timedOut, failed } as independent fields --- matching the repo's own defensive-patterns rule to report orthogonal subprocess outcomes independently. runLoaderSmoke passes input: '' for its stdin-close contract, and sites whose assertions pin exact stream bytes pass stripFinalNewline: false.

## Decision status

Historical design. Use it to understand an older pressure or failure; locate the replacing decision before implementing anything.

- Raw note: [archived/testing/2026-07-26-execa-for-test-subprocess-plumbing.md](../02-notes/archived/testing/2026-07-26-execa-for-test-subprocess-plumbing.md)
- Pinned source: [.agents/notes/archived/testing/2026-07-26-execa-for-test-subprocess-plumbing.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/archived/testing/2026-07-26-execa-for-test-subprocess-plumbing.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`packages/examples/acp-demo/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/examples/acp-demo/src/index.ts) | package entry point | Core file in the package named by the note: `packages/examples/acp-demo`. | `named-package-member` |
| [`packages/examples/acp-demo/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/examples/acp-demo/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/examples/acp-demo`. | `named-package-member` |
| [`packages/test-support/acp-snapshot/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/test-support/acp-snapshot/src/index.ts) | package entry point | Core file in the package named by the note: `packages/test-support/acp-snapshot`. | `named-package-member` |
| [`packages/test-support/acp-snapshot/src/suite.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/test-support/acp-snapshot/src/suite.ts) | runtime implementation | Core file in the package named by the note: `packages/test-support/acp-snapshot`. | `named-package-member` |
| [`packages/test-support/loader-smoke/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/test-support/loader-smoke/src/index.ts) | package entry point | Core file in the package named by the note: `packages/test-support/loader-smoke`. Defines `runLoaderSmoke`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/test-support/llm-mock-server/src/bin.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/test-support/llm-mock-server/src/bin.ts) | runtime implementation | Core file in the package named by the note: `packages/test-support/llm-mock-server`. | `named-package-member` |
| [`packages/test-support/acp-snapshot/src/harness.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/test-support/acp-snapshot/src/harness.ts) | runtime implementation | Core file in the package named by the note: `packages/test-support/acp-snapshot`. Defines `waitForPersistedTurnStart`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/test-support/llm-mock-server/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/test-support/llm-mock-server/src/index.ts) | package entry point | Core file in the package named by the note: `packages/test-support/llm-mock-server`. | `named-package-member` |
| [`packages/test-support/acp-snapshot/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/test-support/acp-snapshot/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/test-support/acp-snapshot`. | `named-package-member` |
| [`packages/test-support/acp-snapshot/src/normalize.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/test-support/acp-snapshot/src/normalize.ts) | runtime implementation | Core file in the package named by the note: `packages/test-support/acp-snapshot`. | `named-package-member` |
| [`packages/test-support/loader-smoke/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/test-support/loader-smoke/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/test-support/loader-smoke`. | `named-package-member` |
| [`packages/test-support/llm-mock-server/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/test-support/llm-mock-server/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/test-support/llm-mock-server`. | `named-package-member` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `parseArgs` | `function` | [`packages/sandbox/sandbox-windows-acl/src/runner.ts:75`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/sandbox/sandbox-windows-acl/src/runner.ts#L75) | `function parseArgs(raw: string[]): ParsedArgs {` |
| `kill` | `const` | [`packages/subprocess/subprocess-local/src/spawn.ts:432`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/subprocess/subprocess-local/src/spawn.ts#L432) | `const kill = (sig: NodeJS.Signals): void => {` |
| `waitForPersistedTurnStart` | `function` | [`packages/test-support/acp-snapshot/src/harness.ts:519`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/test-support/acp-snapshot/src/harness.ts#L519) | `async function waitForPersistedTurnStart(` |
| `waitForPersistedTurnEnd` | `function` | [`packages/test-support/acp-snapshot/src/harness.ts:552`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/test-support/acp-snapshot/src/harness.ts#L552) | `async function waitForPersistedTurnEnd(` |
| `waitForWorkspaceFile` | `function` | [`packages/test-support/acp-snapshot/src/harness.ts:684`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/test-support/acp-snapshot/src/harness.ts#L684) | `async function waitForWorkspaceFile(` |
| `runLoaderSmoke` | `function` | [`packages/test-support/loader-smoke/src/index.ts:174`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/test-support/loader-smoke/src/index.ts#L174) | `export async function runLoaderSmoke(options: LoaderSmokeOptions): Promise<LoaderSmokeResult> {` |

### Tests and executable evidence

- [`apps/web/tests/scaffold.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/scaffold.ts) — The source note names this file directly. Contains the exact code literal `dsh-acp-snapshot` named by the note.
- [`apps/cli/tests/built-bin.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/cli/tests/built-bin.e2e.ts) — The source note names this file directly.
- [`apps/web/tests/smoke-real.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/smoke-real.e2e.ts) — The source note names this file directly.
- [`examples/jsonrpc-agent/tests/keyless-smoke.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/examples/jsonrpc-agent/tests/keyless-smoke.e2e.ts) — The source note names this file directly.
- [`packages/examples/acp-demo/tests/built-bin.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/examples/acp-demo/tests/built-bin.e2e.ts) — The source note names this file directly. A test under the owning area exercises or imports `setEncoding`.
- [`packages/examples/acp-demo/tests/load-path.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/examples/acp-demo/tests/load-path.e2e.ts) — A test under the owning area exercises or imports `setEncoding`. A test under the owning area exercises or imports `acp-demo`.
- [`packages/examples/acp-demo/tests/acp-agent.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/examples/acp-demo/tests/acp-agent.spec.ts) — A test under the owning area exercises or imports `acp-demo`.
- [`packages/test-support/llm-mock-server/tests/cli.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/test-support/llm-mock-server/tests/cli.spec.ts) — A test under the owning area exercises or imports `parseArgs`.

## How to read the implementation

1. Start with [`packages/examples/acp-demo/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/examples/acp-demo/src/index.ts) because it has the strongest evidence link to the note.
2. Read public types and entry points before implementation details. Write down the contract in plain language.
3. Follow the named constructs into their callers and tests. Confirm which component can actually enforce the rule.
4. Reconstruct one failure path: cancellation, crash, retry, stale state, partial write, or incompatible provider input—whichever the note is protecting against.
5. Only then map the contract into the current Rust repository. Target paths must come from that repository, never from guesswork in this wiki.

## Rust harness port

**Likely integration point:** repository tests and engineering policy.

Preserve the behavior and ownership rule. Do not mechanically translate Cordis, TypeScript declaration merging, Node streams, or package topology into Rust.

Relevant standing rules:

- **`rust/append-log`** — Keep durable facts append-only and derive views from them. Separate the synchronous fact boundary from asynchronous persistence.
- **`rust/four-tool-surface`** — Preserve the four model tools: read, write, patch, and exec. Reuse internal capabilities without automatically exposing new model-facing tools.
- **`rust/newtype-ids`** — Represent identity domains with Rust newtypes; do not pass bare strings across session, call, turn, step, and agent boundaries.
- **`rust/process-discipline`** — Treat this as repository or release policy unless it changes a runtime contract. Do not add runtime machinery for a process-only lesson.
- **`rust/provider-adapter`** — Keep provider wire quirks inside adapters; the loop and durable event vocabulary remain provider-neutral.
- **`rust/quickjs-boundary`** — QuickJS holds orchestration variables; Rust/Tokio owns async execution and exposes only explicitly granted capabilities.
- **`rust/serde-validation`** — Parse untrusted input into strict serde types and validate semantic constraints before executing side effects.
- **`rust/tagged-enums`** — Use serde-tagged enums for a closed wire vocabulary. Add an explicit extension envelope only where runtime extension is truly required.
- **`rust/tokio-cancellation`** — Thread one cancellation token through async work, close owned resources before reuse, and test cancellation at every await boundary.
- **`rust/trait-seam`** — Use a small trait at the capability boundary; keep policy and model-facing schemas outside the backend implementation.
- **Status gate** — Historical design. Use it to understand an older pressure or failure; locate the replacing decision before implementing anything.

### Smallest safe implementation slice

1. Express the semantic contract as Rust types and one owning boundary.
2. Add the smallest adapter or service needed by the existing loop; do not add a new model-facing tool unless the four-tool surface cannot express the job.
3. Record durable evidence before deriving summaries or UI state.
4. Add one happy-path test, one interruption test, and one replay/restart test where state survives a turn.
5. Keep the patch reversible until the source note's strongest failure case is proven in the Rust harness.

## Search handles

- Tags: `class/testing`, `concern/boundary`, `concern/cancellation-timeout`, `concern/compatibility`, `concern/evidence`, `concern/human-control`, `concern/lifecycle`, `concern/ownership`, `concern/recovery`, `concern/schema-types`, `concern/simplification`, `domain/build-release`, `domain/configuration`, `domain/llm`, `domain/protocols`, `domain/session-state`, `domain/shell-terminal`, `domain/storage`, `domain/testing`, `domain/ui-interaction`, `lifecycle/archived`, `mechanism/policy`, `mechanism/projection`, `mechanism/streaming`
- Aliases: `parseArgs`, `kill`, `waitForPersistedTurnStart`, `waitForPersistedTurnEnd`, `waitForWorkspaceFile`, `runLoaderSmoke`, `setEncoding`, `setTimeout`, `packages/support/loader-smoke/src/index.ts`, `runBuiltBin`, `apps/cli/tests/built-bin.e2e.ts`, `packages/examples/cli-demo/tests/built-bin.e2e.ts`, `runBinExpectingExit`, `packages/examples/acp-demo/tests/built-bin.e2e.ts`
- Regex: `(?i)(parseArgs|kill|waitForPersistedTurnStart|waitForPersistedTurnEnd|waitForWorkspaceFile|runLoaderSmoke|setEncoding|setTimeout)`

```bash
rg -n --pcre2 "(?i)(parseArgs|kill|waitForPersistedTurnStart|waitForPersistedTurnEnd|waitForWorkspaceFile|runLoaderSmoke|setEncoding|setTimeout)" source-deepseek-harness
rg -l --fixed-strings "class/testing" 03-agent-context
```

## Connected notes

- **`shares-code-with`** — [0682. Extract the ACP snapshot suite into a support package](0682-extract-the-acp-snapshot-suite-into-a-support-package.md): Shares source implementation: `packages/examples/acp-demo/src/index.ts`, `packages/examples/acp-demo/src/invariant.ts`.
- **`shares-code-with`** — [0359. Pre-Plugin Theme Bootstrap](0359-pre-plugin-theme-bootstrap.md): Shares source implementation: `apps/web/tests/scaffold.ts`, `apps/web/tests/smoke-real.e2e.ts`.
- **`shares-code-with`** — [0456. Remove the stdio and Echo agents](0456-remove-the-stdio-and-echo-agents.md): Shares source implementation: `apps/cli/tests/built-bin.e2e.ts`, `packages/examples/acp-demo/src/index.ts`.
- **`shares-code-with`** — [0500. Keyless browser e2e lane for the web GUI](0500-keyless-browser-e2e-lane-for-the-web-gui.md): Shares source implementation: `apps/web/tests/scaffold.ts`.
- **`shares-code-with`** — [0429. Browser GIFs preserve one evidence chain](0429-browser-gifs-preserve-one-evidence-chain.md): Shares source implementation: `apps/cli/tests/built-bin.e2e.ts`, `apps/web/tests/scaffold.ts`.
- **`shares-code-with`** — [0553. Parse `dsh` argv through one Commander adapter](0553-parse-dsh-argv-through-one-commander-adapter.md): Shares source implementation: `apps/cli/tests/built-bin.e2e.ts`, `packages/examples/acp-demo/src/index.ts`.
- **`shares-code-with`** — [0598. Address pending queue occurrences for edit and removal](0598-address-pending-queue-occurrences-for-edit-and-removal.md): Shares source implementation: `apps/web/tests/smoke-real.e2e.ts`.
- **`shares-code-with`** — [0431. Dual Wine and native Windows pull-request CI](0431-dual-wine-and-native-windows-pull-request-ci.md): Shares source implementation: `apps/cli/tests/built-bin.e2e.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0684-adopt-execa-for-hand-rolled-test-subprocess-plumbing.md`.
