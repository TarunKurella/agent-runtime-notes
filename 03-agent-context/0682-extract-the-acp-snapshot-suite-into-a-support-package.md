---
id: "dsh-note-0682"
title: "Extract the ACP snapshot suite into a support package"
status: "archived"
class: "testing"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/archived/testing/2026-07-08-shared-acp-snapshot-package.md"
implementation_evidence: "medium"
target_anchor: "repository tests and engineering policy"
tags:
  - "class/testing"
  - "concern/boundary"
  - "concern/concurrency"
  - "concern/discovery-routing"
  - "concern/evidence"
  - "concern/human-control"
  - "concern/ownership"
  - "concern/recovery"
  - "concern/trust"
  - "domain/configuration"
  - "domain/extensions"
  - "domain/filesystem"
  - "domain/llm"
  - "domain/protocols"
  - "domain/security"
  - "domain/session-state"
  - "domain/shell-terminal"
  - "domain/storage"
  - "domain/testing"
  - "lifecycle/archived"
  - "mechanism/event-sourcing"
  - "mechanism/policy"
  - "mechanism/registry"
  - "mechanism/streaming"
aliases:
  - "json"
  - "optionId"
  - "describe"
  - "mode"
  - "cancelled"
  - "runScenario"
  - "AgentUnderTest"
  - "launchAcpTestAgent"
  - "scrubSystemPrompts"
  - "scrubRequestHeaders"
  - "Scenario"
  - "sessionFixtureNames"
  - "fixtureContext"
  - "normalizedHeaders"
search_regex: "(?i)(json|optionId|describe|mode|cancelled|runScenario|AgentUnderTest|launchAcpTestAgent)"
---

# 0682. Extract the ACP snapshot suite into a support package — implementation context

## Open this when

The ACP snapshot tier (snapshot Agent Note) was built from three modules living inside one example's test directory: snapshot-harness.ts (boot the real bin subprocess, drive it over ACP JSON-RPC, harvest the persisted logs), snapshot-normalize.ts (the pure expected-output normalizers), and the ~150-line scenario body plus fixture guards in acp.snapshot.ts (record/replay modes, the stdout expected-output and log comparisons, the pinned-header uniformity guard, the orphan/required-file/single-pin meta-tests).

## Source decision

The machinery lives in packages/support/acp-snapshot (@deepseek-ai/dsh-acp-snapshot); an example's .snapshot.ts is its scenario table, its agent paths, and one factory call, over its own snapshots/ fixtures and cordis.snapshot.yml overlay (single-source replay config). Reading DSH_SNAPSHOT stays at that edge --- the library takes a resolved mode.

## Decision status

Historical design. Use it to understand an older pressure or failure; locate the replacing decision before implementing anything.

- Raw note: [archived/testing/2026-07-08-shared-acp-snapshot-package.md](../02-notes/archived/testing/2026-07-08-shared-acp-snapshot-package.md)
- Pinned source: [.agents/notes/archived/testing/2026-07-08-shared-acp-snapshot-package.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/archived/testing/2026-07-08-shared-acp-snapshot-package.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`packages/examples/acp-demo/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/examples/acp-demo/src/index.ts) | package entry point | Core file in the package named by the note: `packages/examples/acp-demo`. | `named-package-member` |
| [`packages/examples/acp-demo/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/examples/acp-demo/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/examples/acp-demo`. | `named-package-member` |
| [`packages/test-support/llm-replay/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/test-support/llm-replay/src/index.ts) | package entry point | Core file in the package named by the note: `packages/test-support/llm-replay`. | `named-package-member` |
| [`packages/test-support/acp-snapshot/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/test-support/acp-snapshot/src/index.ts) | package entry point | Core file in the package named by the note: `packages/test-support/acp-snapshot`. | `named-package-member` |
| [`packages/test-support/acp-snapshot/src/suite.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/test-support/acp-snapshot/src/suite.ts) | runtime implementation | Core file in the package named by the note: `packages/test-support/acp-snapshot`. Defines `Scenario`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/test-support/acp-snapshot/src/harness.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/test-support/acp-snapshot/src/harness.ts) | runtime implementation | Core file in the package named by the note: `packages/test-support/acp-snapshot`. Defines `runScenario`, a construct named by the note. | `exact-code-occurrence, named-package-member, symbol-definition` |
| [`packages/test-support/llm-replay/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/test-support/llm-replay/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/test-support/llm-replay`. | `named-package-member` |
| [`packages/test-support/acp-snapshot/src/launcher.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/test-support/acp-snapshot/src/launcher.ts) | runtime implementation | Core file in the package named by the note: `packages/test-support/acp-snapshot`. Defines `launchAcpTestAgent`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/test-support/acp-snapshot/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/test-support/acp-snapshot/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/test-support/acp-snapshot`. | `named-package-member` |
| [`packages/test-support/acp-snapshot/src/normalize.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/test-support/acp-snapshot/src/normalize.ts) | runtime implementation | Core file in the package named by the note: `packages/test-support/acp-snapshot`. Defines `scrubSystemPrompts`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/examples/acp-demo`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/packages/examples/acp-demo) | package or module directory | The note names this package or capability. | `named-package` |
| [`packages/test-support/llm-replay`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/packages/test-support/llm-replay) | package or module directory | The note names this package or capability. | `named-package` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `json` | `const` | [`packages/client/modules/src/index.ts:169`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/modules/src/index.ts#L169) | `const json = JSON.stringify(graph).replaceAll('<', '\\u003c')` |
| `optionId` | `function` | [`packages/client/ui-input-trigger/src/client/MenuView.tsx:25`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-input-trigger/src/client/MenuView.tsx#L25) | `function optionId(source: string, index: number): string {` |
| `describe` | `function` | [`packages/core/tools/src/py-types.ts:223`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/py-types.ts#L223) | `function describe(schema: object): string \| undefined {` |
| `mode` | `const` | [`packages/e2b/fs-e2b/src/index.ts:535`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/e2b/fs-e2b/src/index.ts#L535) | `const mode = existing === undefined ? 0o600 : existing.mode & 0o777` |
| `cancelled` | `const` | [`packages/host/apiproxy/src/api-proxy.ts:2037`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/src/api-proxy.ts#L2037) | `const cancelled = () => err<{ items: SessionSearchItem[]; hasMore: boolean }>(request, {` |
| `runScenario` | `function` | [`packages/test-support/acp-snapshot/src/harness.ts:225`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/test-support/acp-snapshot/src/harness.ts#L225) | `export async function runScenario(input: InputScript, opts: RunOptions): Promise<RunResult> {` |
| `AgentUnderTest` | `interface` | [`packages/test-support/acp-snapshot/src/launcher.ts:27`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/test-support/acp-snapshot/src/launcher.ts#L27) | `export interface AgentUnderTest {` |
| `launchAcpTestAgent` | `function` | [`packages/test-support/acp-snapshot/src/launcher.ts:78`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/test-support/acp-snapshot/src/launcher.ts#L78) | `export function launchAcpTestAgent(options: AcpTestLaunchOptions): LaunchedAcpTestAgent {` |
| `scrubSystemPrompts` | `function` | [`packages/test-support/acp-snapshot/src/normalize.ts:343`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/test-support/acp-snapshot/src/normalize.ts#L343) | `export function scrubSystemPrompts(rawLog: string): string {` |
| `scrubRequestHeaders` | `function` | [`packages/test-support/acp-snapshot/src/normalize.ts:372`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/test-support/acp-snapshot/src/normalize.ts#L372) | `export function scrubRequestHeaders(rawLog: string): string {` |
| `Scenario` | `interface` | [`packages/test-support/acp-snapshot/src/suite.ts:66`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/test-support/acp-snapshot/src/suite.ts#L66) | `export interface Scenario {` |
| `sessionFixtureNames` | `function` | [`packages/test-support/acp-snapshot/src/suite.ts:334`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/test-support/acp-snapshot/src/suite.ts#L334) | `export function sessionFixtureNames(names: readonly string[]): string[] {` |
| `fixtureContext` | `function` | [`packages/test-support/acp-snapshot/src/suite.ts:368`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/test-support/acp-snapshot/src/suite.ts#L368) | `export function fixtureContext(fixture: string): NormalizeContext {` |
| `normalizedHeaders` | `function` | [`packages/test-support/acp-snapshot/src/suite.ts:388`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/test-support/acp-snapshot/src/suite.ts#L388) | `export function normalizedHeaders(rawLog: string, ctx: NormalizeContext): unknown[] {` |
| `normalizedSystemPrompts` | `function` | [`packages/test-support/acp-snapshot/src/suite.ts:406`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/test-support/acp-snapshot/src/suite.ts#L406) | `export function normalizedSystemPrompts(rawLog: string, ctx: NormalizeContext): string[] {` |
| `formatSystemPromptSnapshot` | `function` | [`packages/test-support/acp-snapshot/src/suite.ts:494`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/test-support/acp-snapshot/src/suite.ts#L494) | `export function formatSystemPromptSnapshot(` |

### Tests and executable evidence

- [`packages/typert/generator/tests/fixtures/type-model/packages/client/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/typert/generator/tests/fixtures/type-model/packages/client/src/index.ts) — Core file in the package named by the note: `packages/typert/generator/tests/fixtures/type-model/packages/client`.
- [`examples/acp-agent/tests/snapshots`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/examples/acp-agent/tests/snapshots) — The source note names this implementation area directly.
- [`packages/typert/generator/tests/fixtures/type-model/packages/client`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/typert/generator/tests/fixtures/type-model/packages/client) — The note names this package or capability.
- [`packages/typert/generator/tests/fixtures/type-model/packages/client/package.json`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/typert/generator/tests/fixtures/type-model/packages/client/package.json) — Core file in the package named by the note: `packages/typert/generator/tests/fixtures/type-model/packages/client`.
- [`packages/examples/acp-demo/tests/load-path.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/examples/acp-demo/tests/load-path.e2e.ts) — A test under the owning area exercises or imports `TSX_TSCONFIG_PATH`. A test under the owning area exercises or imports `binScript`.
- [`packages/examples/acp-demo/tests/built-bin.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/examples/acp-demo/tests/built-bin.e2e.ts) — A test under the owning area exercises or imports `dsh-acp-demo`.
- [`packages/typert/generator/tests/type-model.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/typert/generator/tests/type-model.spec.ts) — A test under the owning area exercises or imports `configPath`.
- [`packages/examples/acp-demo/tests/acp-agent.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/examples/acp-demo/tests/acp-agent.spec.ts) — A test under the owning area exercises or imports `dsh-acp-demo`.
- Source verification intent: Extraction parity was proven mechanically: after the move, pnpm run test:snapshot matched the base commit's result with zero byte changes under examples/acp-agent/tests/snapshots/. The package's src/ holds per-file 100% statements/branches/functions/lines under the gating unit run, driven through the real launcher by a scripted fake ACP bin (tests/fixtures/fake-acp-agent.ts, behavior scripted per scenario via a behavior.json beside the fixture): harness.spec.ts directly covers launcher defaults, captures, update waiting, shutdown, and environment/config variants, then covers every scenario step op, both.

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
- **`rust/capability-security`** — Make granted capabilities explicit. On the Windows no-admin path, probe support at runtime and fail closed rather than silently running unsandboxed.
- **`rust/four-tool-surface`** — Preserve the four model tools: read, write, patch, and exec. Reuse internal capabilities without automatically exposing new model-facing tools.
- **`rust/process-discipline`** — Treat this as repository or release policy unless it changes a runtime contract. Do not add runtime machinery for a process-only lesson.
- **`rust/provider-adapter`** — Keep provider wire quirks inside adapters; the loop and durable event vocabulary remain provider-neutral.
- **`rust/quickjs-boundary`** — QuickJS holds orchestration variables; Rust/Tokio owns async execution and exposes only explicitly granted capabilities.
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

- Tags: `class/testing`, `concern/boundary`, `concern/concurrency`, `concern/discovery-routing`, `concern/evidence`, `concern/human-control`, `concern/ownership`, `concern/recovery`, `concern/trust`, `domain/configuration`, `domain/extensions`, `domain/filesystem`, `domain/llm`, `domain/protocols`, `domain/security`, `domain/session-state`, `domain/shell-terminal`, `domain/storage`, `domain/testing`, `lifecycle/archived`, `mechanism/event-sourcing`, `mechanism/policy`, `mechanism/registry`, `mechanism/streaming`
- Aliases: `json`, `optionId`, `describe`, `mode`, `cancelled`, `runScenario`, `AgentUnderTest`, `launchAcpTestAgent`, `scrubSystemPrompts`, `scrubRequestHeaders`, `Scenario`, `sessionFixtureNames`, `fixtureContext`, `normalizedHeaders`
- Regex: `(?i)(json|optionId|describe|mode|cancelled|runScenario|AgentUnderTest|launchAcpTestAgent)`

```bash
rg -n --pcre2 "(?i)(json|optionId|describe|mode|cancelled|runScenario|AgentUnderTest|launchAcpTestAgent)" source-deepseek-harness
rg -l --fixed-strings "class/testing" 03-agent-context
```

## Connected notes

- **`source-link`** — [0680. Single-source the acp-agent replay config](0680-single-source-the-acp-agent-replay-config.md): The source note links to this decision directly.
- **`source-link`** — [0681. Pin request-header content in one snapshot scenario](0681-pin-request-header-content-in-one-snapshot-scenario.md): The source note links to this decision directly.
- **`shares-code-with`** — [0684. Adopt execa for hand-rolled test subprocess plumbing](0684-adopt-execa-for-hand-rolled-test-subprocess-plumbing.md): Shares source implementation: `packages/examples/acp-demo/src/index.ts`, `packages/examples/acp-demo/src/invariant.ts`.
- **`shares-code-with`** — [0495. ACP snapshot tests --- record-once / replay-deterministic](0495-acp-snapshot-tests-record-once-replay-deterministic.md): Shares source implementation: `packages/test-support/llm-replay/src/index.ts`, `packages/test-support/llm-replay/src/invariant.ts`.
- **`shares-code-with`** — [0677. Use `session.jsonl` as the only snapshot session-log artifact](0677-use-session-jsonl-as-the-only-snapshot-session-log-artifact.md): Shares source implementation: `packages/test-support/acp-snapshot/src/normalize.ts`, `packages/test-support/acp-snapshot/src/suite.ts`.
- **`shares-code-with`** — [0280. Parallel subagent delegations](0280-parallel-subagent-delegations.md): Shares source implementation: `packages/test-support/llm-replay/src/index.ts`, `packages/test-support/llm-replay/src/invariant.ts`.
- **`shares-code-with`** — [0128. A pi-ai model declares its own input modalities, and undeclared means text](0128-a-pi-ai-model-declares-its-own-input-modalities-and-undeclared-means-tex.md): Shares source implementation: `packages/test-support/llm-replay/src/index.ts`, `packages/test-support/llm-replay/src/invariant.ts`.
- **`shares-code-with`** — [0683. Snapshot semantic terminal state for the TUI](0683-snapshot-semantic-terminal-state-for-the-tui.md): Shares source implementation: `packages/test-support/llm-replay/src/index.ts`, `packages/test-support/llm-replay/src/invariant.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0682-extract-the-acp-snapshot-suite-into-a-support-package.md`.
