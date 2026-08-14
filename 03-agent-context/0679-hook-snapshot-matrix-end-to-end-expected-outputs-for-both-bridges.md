---
id: "dsh-note-0679"
title: "Hook snapshot matrix --- end-to-end expected outputs for both bridges"
status: "archived"
class: "testing"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/archived/testing/2026-07-04-hook-snapshot-matrix.md"
implementation_evidence: "high"
target_anchor: "repository tests and engineering policy"
tags:
  - "class/testing"
  - "concern/boundary"
  - "concern/concurrency"
  - "concern/evidence"
  - "concern/human-control"
  - "concern/lifecycle"
  - "concern/ownership"
  - "concern/recovery"
  - "domain/configuration"
  - "domain/extensions"
  - "domain/filesystem"
  - "domain/llm"
  - "domain/multi-agent"
  - "domain/protocols"
  - "domain/session-state"
  - "domain/shell-terminal"
  - "domain/storage"
  - "domain/testing"
  - "domain/tools"
  - "lifecycle/archived"
  - "mechanism/event-sourcing"
  - "mechanism/policy"
aliases:
  - "apply"
  - "logger"
  - "json"
  - "durationMs"
  - "rejected"
  - "dsh-hooks-claude"
  - "dsh-hooks-codex"
  - "hooks.e2e.ts"
  - "PreToolUse"
  - "acp-agent"
  - "UserPromptSubmit"
  - "hook-cc-promptsubmit-block"
  - "examples/acp-agent/cordis.yml"
  - "cordis.snapshot.yml"
search_regex: "(?i)(apply|logger|json|durationMs|rejected|dsh\\-hooks\\-claude|dsh\\-hooks\\-codex|hooks\\.e2e\\.ts)"
---

# 0679. Hook snapshot matrix --- end-to-end expected outputs for both bridges — implementation context

## Open this when

The hook bridges --- dsh-hooks-claude (7 Claude Code hook points) and dsh-hooks-codex (5 Codex points) --- map external hook commands onto the harness interception seams. They carry deep unit and coverage-spec coverage (every decision arm, every payload dialect, driven against a mocked seam) plus one key-gated e2e (hooks.e2e.ts, a live PreToolUse block).

## Source decision

The implementation has two coupled parts: examples/acp-agent/cordis.yml and cordis.snapshot.yml now load dsh-hooks-codex alongside dsh-hooks-claude, each pointed at its own config file (./hooks.json for Claude, ./codex-hooks.json for Codex --- the two dialects cannot share one file). This is a genuine product-surface change, not test-only wiring: the shipped ACP server (and the demo:acp front door) now carries both bridges.

## Decision status

Historical design. Use it to understand an older pressure or failure; locate the replacing decision before implementing anything.

- Raw note: [archived/testing/2026-07-04-hook-snapshot-matrix.md](../02-notes/archived/testing/2026-07-04-hook-snapshot-matrix.md)
- Pinned source: [.agents/notes/archived/testing/2026-07-04-hook-snapshot-matrix.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/archived/testing/2026-07-04-hook-snapshot-matrix.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`examples/acp-agent/cordis.yml`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/examples/acp-agent/cordis.yml) | composition and configuration | The source note names this file directly. | `named-file` |
| [`packages/hooks/hooks-codex/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/hooks/hooks-codex/src/index.ts) | package entry point | Entry point or contract under the directory named by the note: `packages/hooks/hooks-codex`. Core file in the package named by the note: `packages/hooks/hooks-codex`. | `named-directory-member, named-package-member, symbol-definition` |
| [`packages/hooks/hooks-codex/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/hooks/hooks-codex/src/invariant.ts) | runtime contract checks | Entry point or contract under the directory named by the note: `packages/hooks/hooks-codex`. Core file in the package named by the note: `packages/hooks/hooks-codex`. | `named-directory-member, named-package-member, symbol-definition` |
| [`packages/hooks/hooks-codex/README.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/hooks/hooks-codex/README.md) | package contract and examples | Entry point or contract under the directory named by the note: `packages/hooks/hooks-codex`. Core file in the package named by the note: `packages/hooks/hooks-codex`. | `named-directory-member, named-package-member` |
| [`packages/hooks/hooks-codex/package.json`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/hooks/hooks-codex/package.json) | composition and configuration | Entry point or contract under the directory named by the note: `packages/hooks/hooks-codex`. Core file in the package named by the note: `packages/hooks/hooks-codex`. | `named-directory-member, named-package-member` |
| [`packages/hooks/hooks-codex`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/packages/hooks/hooks-codex) | package or module directory | The source note names this implementation area directly. The note names this package or capability. | `named-directory, named-package` |
| [`examples/acp-agent/pty-snapshot-backend.mjs`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/examples/acp-agent/pty-snapshot-backend.mjs) | runtime implementation | Core file in the package named by the note: `examples/acp-agent`. Defines `apply`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`examples/acp-agent/web-fetch-fixture-server.mjs`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/examples/acp-agent/web-fetch-fixture-server.mjs) | runtime implementation | Core file in the package named by the note: `examples/acp-agent`. Defines `apply`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`examples/acp-agent`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/examples/acp-agent) | package or module directory | The note names this package or capability. | `named-package` |
| [`packages/acp/acp/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/acp/acp/src/index.ts) | package entry point | Defines `logger`, a construct named by the note. | `symbol-definition` |
| [`packages/client/modules/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/modules/src/index.ts) | package entry point | Defines `json`, a construct named by the note. | `symbol-definition` |
| [`packages/host/apiproxy/src/api-proxy.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/src/api-proxy.ts) | runtime implementation | Defines `rejected`, a construct named by the note. | `symbol-definition` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `apply` | `function` | [`examples/acp-agent/pty-snapshot-backend.mjs:59`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/examples/acp-agent/pty-snapshot-backend.mjs#L59) | `export function apply(ctx) {` |
| `apply` | `function` | [`examples/acp-agent/web-fetch-fixture-server.mjs:32`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/examples/acp-agent/web-fetch-fixture-server.mjs#L32) | `export async function apply(ctx) {` |
| `logger` | `const` | [`packages/acp/acp/src/index.ts:109`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/acp/acp/src/index.ts#L109) | `const logger = ctx.logger` |
| `json` | `const` | [`packages/client/modules/src/index.ts:169`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/modules/src/index.ts#L169) | `const json = JSON.stringify(graph).replaceAll('<', '\\u003c')` |
| `durationMs` | `const` | [`packages/client/ui-trajectory/src/client/timeline.ts:58`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-trajectory/src/client/timeline.ts#L58) | `const durationMs = finite(cell.timeSeconds)` |
| `apply` | `function` | [`packages/hooks/hooks-codex/src/index.ts:81`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/hooks/hooks-codex/src/index.ts#L81) | `export function apply(ctx: Context, config: Config): void {` |
| `apply` | `const` | [`packages/hooks/hooks-codex/src/invariant.ts:28`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/hooks/hooks-codex/src/invariant.ts#L28) | `export const apply = (ctx: Context): Promise<() => void> =>` |
| `rejected` | `let` | [`packages/host/apiproxy/src/api-proxy.ts:1768`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/src/api-proxy.ts#L1768) | `let rejected = false` |

### Tests and executable evidence

- [`examples/acp-agent/tests/snapshots`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/examples/acp-agent/tests/snapshots) — The source note names this implementation area directly.
- [`examples/acp-agent/tests/hooks.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/examples/acp-agent/tests/hooks.e2e.ts) — A test under the owning area exercises or imports `PreToolUse`.
- [`examples/acp-agent/tests/acp.snapshot.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/examples/acp-agent/tests/acp.snapshot.ts) — A test under the owning area exercises or imports `hook-cc-promptsubmit-block`. A test under the owning area exercises or imports `hook-codex-promptsubmit-block`.
- [`packages/hooks/hooks-codex/tests/bridge.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/hooks/hooks-codex/tests/bridge.spec.ts) — A test under the owning area exercises or imports `dsh-hooks-codex`. A test under the owning area exercises or imports `PreToolUse`.
- [`packages/hooks/hooks-codex/tests/config.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/hooks/hooks-codex/tests/config.spec.ts) — A test under the owning area exercises or imports `dsh-hooks-codex`. A test under the owning area exercises or imports `PreToolUse`.
- [`packages/hooks/hooks-codex/tests/coverage-cases.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/hooks/hooks-codex/tests/coverage-cases.ts) — A test under the owning area exercises or imports `dsh-hooks-codex`. A test under the owning area exercises or imports `PreToolUse`.
- [`examples/acp-agent/tests/snapshots/packed-chunks/session.jsonl`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/examples/acp-agent/tests/snapshots/packed-chunks/session.jsonl) — A test under the owning area exercises or imports `PreToolUse`. A test under the owning area exercises or imports `durationMs`.
- [`examples/acp-agent/tests/snapshots/hook-cc-pretool-ask/session.jsonl`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/examples/acp-agent/tests/snapshots/hook-cc-pretool-ask/session.jsonl) — A test under the owning area exercises or imports `PreToolUse`. A test under the owning area exercises or imports `durationMs`.

## How to read the implementation

1. Start with [`examples/acp-agent/cordis.yml`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/examples/acp-agent/cordis.yml) because it has the strongest evidence link to the note.
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

- Tags: `class/testing`, `concern/boundary`, `concern/concurrency`, `concern/evidence`, `concern/human-control`, `concern/lifecycle`, `concern/ownership`, `concern/recovery`, `domain/configuration`, `domain/extensions`, `domain/filesystem`, `domain/llm`, `domain/multi-agent`, `domain/protocols`, `domain/session-state`, `domain/shell-terminal`, `domain/storage`, `domain/testing`, `domain/tools`, `lifecycle/archived`, `mechanism/event-sourcing`, `mechanism/policy`
- Aliases: `apply`, `logger`, `json`, `durationMs`, `rejected`, `dsh-hooks-claude`, `dsh-hooks-codex`, `hooks.e2e.ts`, `PreToolUse`, `acp-agent`, `UserPromptSubmit`, `hook-cc-promptsubmit-block`, `examples/acp-agent/cordis.yml`, `cordis.snapshot.yml`
- Regex: `(?i)(apply|logger|json|durationMs|rejected|dsh\-hooks\-claude|dsh\-hooks\-codex|hooks\.e2e\.ts)`

```bash
rg -n --pcre2 "(?i)(apply|logger|json|durationMs|rejected|dsh\\-hooks\\-claude|dsh\\-hooks\\-codex|hooks\\.e2e\\.ts)" source-deepseek-harness
rg -l --fixed-strings "class/testing" 03-agent-context
```

## Connected notes

- **`source-link`** — [0680. Single-source the acp-agent replay config](0680-single-source-the-acp-agent-replay-config.md): The source note links to this decision directly.
- **`shares-code-with`** — [0596. `dsh migrate`/`dsh upgrade` seed the first turn with a skill](0596-dsh-migrate-dsh-upgrade-seed-the-first-turn-with-a-skill.md): Shares source implementation: `packages/hooks/hooks-codex`, `packages/hooks/hooks-codex/src/index.ts`.
- **`shares-code-with`** — [0139. dsh-hook-protocol --- the shared Claude Code / Codex hook wire-protocol core](0139-dsh-hook-protocol-the-shared-claude-code-codex-hook-wire-protocol-core.md): Shares source implementation: `packages/hooks/hooks-codex/src/index.ts`, `packages/hooks/hooks-codex/src/invariant.ts`.
- **`shares-code-with`** — [0155. Cross-family file sandbox --- one policy home, a sandboxed fs provider, and fs escalation parity](0155-cross-family-file-sandbox-one-policy-home-a-sandboxed-fs-provider-and-fs.md): Shares source implementation: `examples/acp-agent/cordis.yml`.
- **`same-design-pressure`** — [0557. Agent Client Protocol (ACP) support --- drive the coding agent from external editors](0557-agent-client-protocol-acp-support-drive-the-coding-agent-from-external-e.md): Shares design concerns: `concern/boundary`, `concern/concurrency`, `concern/evidence`.
- **`shares-code-with`** — [0453. Tighten the hook-protocol contract --- dialect, discarded fields, double defaults, and lib-owned `hook/result` semantics](0453-tighten-the-hook-protocol-contract-dialect-discarded-fields-double-defau.md): Shares source implementation: `packages/hooks/hooks-codex/src/index.ts`.
- **`same-design-pressure`** — [0157. Harness-level goal-based execution](0157-harness-level-goal-based-execution.md): Shares design concerns: `concern/boundary`, `concern/concurrency`, `concern/evidence`.
- **`same-design-pressure`** — [0146. The subprocess sandbox --- confinement seam, native runners, escalation, and per-session modes](0146-the-subprocess-sandbox-confinement-seam-native-runners-escalation-and-pe.md): Shares design concerns: `concern/boundary`, `concern/concurrency`, `concern/evidence`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0679-hook-snapshot-matrix-end-to-end-expected-outputs-for-both-bridges.md`.
