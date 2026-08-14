---
id: "dsh-note-0249"
title: "Claude Code and Codex subagent backends"
status: "implemented"
class: "feature"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/feature/2026-08-04-claude-code-and-codex-subagent-backends.md"
implementation_evidence: "medium"
target_anchor: "sandbox capability boundary"
tags:
  - "class/feature"
  - "concern/boundary"
  - "concern/cancellation-timeout"
  - "concern/compatibility"
  - "concern/concurrency"
  - "concern/discovery-routing"
  - "concern/evidence"
  - "concern/human-control"
  - "concern/lifecycle"
  - "concern/ownership"
  - "concern/performance"
  - "concern/recovery"
  - "concern/schema-types"
  - "concern/trust"
  - "domain/agent-loop"
  - "domain/build-release"
  - "domain/configuration"
  - "domain/extensions"
  - "domain/filesystem"
  - "domain/llm"
  - "domain/multi-agent"
  - "domain/observability"
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
  - "mechanism/registry"
  - "mechanism/streaming"
aliases:
  - "subagents"
  - "cancel"
  - "decline"
  - "query"
  - "initialized"
  - "env"
  - "aborted"
  - "SubagentRun"
  - "MAX_TIMER_DELAY_MS"
  - "initialize"
  - "ctx.subagents"
  - "claude-code"
  - "inheritsParentContext: false"
  - "maxDepth: 'provider-managed"
search_regex: "(?i)(subagents|cancel|decline|query|initialized|aborted|SubagentRun|MAX_TIMER_DELAY_MS)"
---

# 0249. Claude Code and Codex subagent backends — implementation context

## Open this when

The named ctx.subagents registry lets a parent agent delegate work without knowing how the child runs, but the harness needs first-party routes to the real Codex and Claude Code products. Each route must hand the product one self-contained task, let it work in the parent Session's workspace, return a final answer or an explicit failure or cancellation, and leave no managed product process behind. The product integrations must not become second owners for task text, cwd, cancellation, result settlement, or process trees.

## Source decision

The harness publishes two sibling one-shot provider packages: codex and claude-code. This note owns their product protocols, result mapping, and process lifecycle; the shared-profile-host placement decision supersedes the original opt-in composition placement. Loading either provider starts no product process, and each tool accepts only a standalone text task; product selection and background execution are not model arguments. Both providers report inheritsParentContext: false, advertise no optional start capabilities, and pass the parent Session cwd without copying the parent conversation.

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/feature/2026-08-04-claude-code-and-codex-subagent-backends.md](../02-notes/implemented/feature/2026-08-04-claude-code-and-codex-subagent-backends.md)
- Pinned source: [.agents/notes/implemented/feature/2026-08-04-claude-code-and-codex-subagent-backends.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/feature/2026-08-04-claude-code-and-codex-subagent-backends.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`THIRD_PARTY_NOTICES.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/THIRD_PARTY_NOTICES.md) | package contract and examples | The source note names this file directly. | `named-file` |
| [`packages/subagent/subagent-codex/src/run.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/subagent/subagent-codex/src/run.ts) | runtime implementation | Core file in the package named by the note: `packages/subagent/subagent-codex`. | `named-package-member` |
| [`packages/subprocess/subprocess/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/subprocess/subprocess/src/index.ts) | package entry point | Core file in the package named by the note: `packages/subprocess/subprocess`. Defines `env`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/subprocess/subprocess/src/types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/subprocess/subprocess/src/types.ts) | public types and contract | Core file in the package named by the note: `packages/subprocess/subprocess`. | `named-package-member` |
| [`packages/subagent/subagent-codex/src/wire.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/subagent/subagent-codex/src/wire.ts) | runtime implementation | Core file in the package named by the note: `packages/subagent/subagent-codex`. Defines `aborted`, a construct named by the note. | `exact-code-occurrence, named-package-member, symbol-definition` |
| [`packages/subagent/tool-subagent/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/subagent/tool-subagent/src/index.ts) | package entry point | Core file in the package named by the note: `packages/subagent/tool-subagent`. | `named-package-member` |
| [`packages/subagent/subagent-codex/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/subagent/subagent-codex/src/index.ts) | package entry point | Core file in the package named by the note: `packages/subagent/subagent-codex`. | `named-package-member` |
| [`packages/subprocess/subprocess/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/subprocess/subprocess/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/subprocess/subprocess`. | `named-package-member` |
| [`packages/subagent/tool-subagent/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/subagent/tool-subagent/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/subagent/tool-subagent`. | `named-package-member` |
| [`packages/subagent/subagent-claude-code/src/run.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/subagent/subagent-claude-code/src/run.ts) | runtime implementation | Core file in the package named by the note: `packages/subagent/subagent-claude-code`. | `named-package-member` |
| [`packages/subagent/subagent-codex/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/subagent/subagent-codex/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/subagent/subagent-codex`. | `named-package-member` |
| [`packages/subagent/subagent-claude-code/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/subagent/subagent-claude-code/src/index.ts) | package entry point | Core file in the package named by the note: `packages/subagent/subagent-claude-code`. | `named-package-member` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `subagents` | `const` | [`packages/acp/acp/src/index.ts:376`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/acp/acp/src/index.ts#L376) | `const subagents = ctx.get('subagents') as ContinuableDrain \| undefined` |
| `cancel` | `const` | [`packages/client/ui-primitives/src/pointer-grace.ts:36`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-primitives/src/pointer-grace.ts#L36) | `const cancel = useCallback(() => {` |
| `decline` | `const` | [`packages/client/ui-user-questions/src/client/contract/slots.ts:77`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-user-questions/src/client/contract/slots.ts#L77) | `const decline = options.find(option => option.label !== intent.approve)` |
| `query` | `const` | [`packages/examples/acp-demo/src/index.ts:134`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/examples/acp-demo/src/index.ts#L134) | `const query = ctx.plugin(SqliteSessionQueryEngine, { path: join(persistenceRoot, 'session-query.db') })` |
| `initialized` | `let` | [`packages/sandbox/sandbox-windows-acl/src/runner.ts:143`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/sandbox/sandbox-windows-acl/src/runner.ts#L143) | `let initialized = false` |
| `env` | `const` | [`packages/subagent/subagent-claude-code/src/process.ts:61`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/subagent/subagent-claude-code/src/process.ts#L61) | `const env = sdkEnvironmentOverlay(options.env)` |
| `aborted` | `const` | [`packages/subagent/subagent-codex/src/wire.ts:67`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/subagent/subagent-codex/src/wire.ts#L67) | `const aborted = new Promise<never>((_resolve, reject) => { rejectAbort = reject })` |
| `SubagentRun` | `interface` | [`packages/subagent/subagent/src/types.ts:249`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/subagent/subagent/src/types.ts#L249) | `export interface SubagentRun {` |
| `env` | `const` | [`packages/subprocess/subprocess/src/index.ts:61`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/subprocess/subprocess/src/index.ts#L61) | `const env: Record<string, string> = {}` |
| `MAX_TIMER_DELAY_MS` | `const` | [`packages/util/timeout/src/index.ts:25`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/util/timeout/src/index.ts#L25) | `export const MAX_TIMER_DELAY_MS = 2_147_483_647` |
| `initialize` | `def` | [`python/sdk/src/deepseek_harness/client.py:117`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/python/sdk/src/deepseek_harness/client.py#L117) | `def initialize(` |

### Tests and executable evidence

- [`python/sdk/tests/test_client.py`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/python/sdk/tests/test_client.py) — A test under the owning area exercises or imports `initialize`.
- [`python/sdk/tests/test_bundled_runtime.py`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/python/sdk/tests/test_bundled_runtime.py) — A test under the owning area exercises or imports `initialize`.
- [`packages/util/timeout/tests/timeout.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/util/timeout/tests/timeout.spec.ts) — A test under the owning area exercises or imports `MAX_TIMER_DELAY_MS`.
- [`packages/subagent/subagent/tests/service.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/subagent/subagent/tests/service.spec.ts) — A test under the owning area exercises or imports `SubagentRun`.
- [`packages/subprocess/subprocess/tests/service.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/subprocess/subprocess/tests/service.spec.ts) — A test under the owning area exercises or imports `dsh-subprocess`.
- [`packages/subagent/tool-subagent/tests/scripted-provider.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/subagent/tool-subagent/tests/scripted-provider.ts) — A test under the owning area exercises or imports `SubagentRun`.
- [`packages/subagent/subagent-codex/tests/real-deepseek.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/subagent/subagent-codex/tests/real-deepseek.e2e.ts) — A test under the owning area exercises or imports `codex`. A test under the owning area exercises or imports `disposeGraceMs`.
- [`packages/subagent/subagent-codex/tests/real-product.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/subagent/subagent-codex/tests/real-product.spec.ts) — A test under the owning area exercises or imports `codex`. A test under the owning area exercises or imports `disposeGraceMs`.

## How to read the implementation

1. Start with [`THIRD_PARTY_NOTICES.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/THIRD_PARTY_NOTICES.md) because it has the strongest evidence link to the note.
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

- Tags: `class/feature`, `concern/boundary`, `concern/cancellation-timeout`, `concern/compatibility`, `concern/concurrency`, `concern/discovery-routing`, `concern/evidence`, `concern/human-control`, `concern/lifecycle`, `concern/ownership`, `concern/performance`, `concern/recovery`, `concern/schema-types`, `concern/trust`, `domain/agent-loop`, `domain/build-release`, `domain/configuration`, `domain/extensions`, `domain/filesystem`, `domain/llm`, `domain/multi-agent`, `domain/observability`, `domain/protocols`, `domain/security`, `domain/session-state`, `domain/shell-terminal`, `domain/testing`, `domain/ui-interaction`, `lifecycle/implemented`, `mechanism/adapter`, `mechanism/policy`, `mechanism/projection`, `mechanism/registry`, `mechanism/streaming`
- Aliases: `subagents`, `cancel`, `decline`, `query`, `initialized`, `env`, `aborted`, `SubagentRun`, `MAX_TIMER_DELAY_MS`, `initialize`, `ctx.subagents`, `claude-code`, `inheritsParentContext: false`, `maxDepth: 'provider-managed`
- Regex: `(?i)(subagents|cancel|decline|query|initialized|aborted|SubagentRun|MAX_TIMER_DELAY_MS)`

```bash
rg -n --pcre2 "(?i)(subagents|cancel|decline|query|initialized|aborted|SubagentRun|MAX_TIMER_DELAY_MS)" source-deepseek-harness
rg -l --fixed-strings "class/feature" 03-agent-context
```

## Connected notes

- **`source-link`** — [0120. Product subagent providers live in the shared profile host](0120-product-subagent-providers-live-in-the-shared-profile-host.md): The source note links to this decision directly.
- **`source-link`** — [0134. Subagent capability seam](0134-subagent-capability-seam.md): The source note links to this decision directly.
- **`shares-code-with`** — [0147. MCP client plugin --- connect to external MCP servers and bridge their tools](0147-mcp-client-plugin-connect-to-external-mcp-servers-and-bridge-their-tools.md): Shares source implementation: `packages/subagent/tool-subagent/src/index.ts`.
- **`shares-code-with`** — [0280. Parallel subagent delegations](0280-parallel-subagent-delegations.md): Shares source implementation: `packages/subagent/tool-subagent/src/index.ts`, `packages/subagent/tool-subagent/src/invariant.ts`.
- **`shares-code-with`** — [0148. Background subagent tasks](0148-background-subagent-tasks.md): Shares source implementation: `packages/subagent/tool-subagent/src/index.ts`, `packages/subagent/tool-subagent/src/invariant.ts`.
- **`shares-code-with`** — [0290. Background job completion wakes an idle owner](0290-background-job-completion-wakes-an-idle-owner.md): Shares source implementation: `packages/subagent/tool-subagent/src/index.ts`, `packages/subagent/tool-subagent/src/invariant.ts`.
- **`shares-code-with`** — [0091. Packaged ripgrep spawn for glob/grep](0091-packaged-ripgrep-spawn-for-glob-grep.md): Shares source implementation: `packages/subprocess/subprocess/src/index.ts`, `packages/subprocess/subprocess/src/types.ts`.
- **`same-design-pressure`** — [0146. The subprocess sandbox --- confinement seam, native runners, escalation, and per-session modes](0146-the-subprocess-sandbox-confinement-seam-native-runners-escalation-and-pe.md): Shares design concerns: `concern/boundary`, `concern/compatibility`, `concern/concurrency`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0249-claude-code-and-codex-subagent-backends.md`.
