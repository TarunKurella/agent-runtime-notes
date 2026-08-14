---
id: "dsh-note-0541"
title: "Use node:timers/promises for hand-rolled cancellable sleeps"
status: "rejected"
class: "simplification"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/rejected/simplification/2026-07-26-builtin-timer-promises-for-hand-rolled-sleeps.md"
implementation_evidence: "high"
target_anchor: "Rust provider-adapter boundary"
tags:
  - "class/simplification"
  - "concern/boundary"
  - "concern/cancellation-timeout"
  - "concern/evidence"
  - "concern/lifecycle"
  - "concern/recovery"
  - "concern/simplification"
  - "domain/build-release"
  - "domain/llm"
  - "domain/protocols"
  - "domain/session-state"
  - "domain/shell-terminal"
  - "domain/storage"
  - "domain/testing"
  - "lifecycle/rejected"
  - "mechanism/projection"
aliases:
  - "cancellableDelay"
  - "pause"
  - "sleep"
  - "delay"
  - "node:timers/promises"
  - "dsh-llm-mock-server"
  - "dsh-lsp-stdio"
  - "dsh-acp-snapshot"
  - "packages/llm/llm-retry/src/index.ts"
  - "new Promise"
  - "setTimeout"
  - "packages/workflow/workflow-worker-thread/src/host.ts"
  - "packages/terminal/terminal-bash/src/session.ts"
  - "import { setTimeout } from 'node:timers/promises"
search_regex: "(?i)(cancellableDelay|pause|sleep|delay|node:timers/promises|dsh\\-llm\\-mock\\-server|dsh\\-lsp\\-stdio|dsh\\-acp\\-snapshot)"
---

# 0541. Use node:timers/promises for hand-rolled cancellable sleeps — implementation context

## Open this when

Three packages hand-roll promise-wrapped timers that the node:timers/promises builtin already provides, while other packages (dsh-llm-mock-server pause(), dsh-lsp-stdio, dsh-acp-snapshot) already use the builtin --- so the hand-rolled copies are also a consistency gap: packages/llm/llm-retry/src/index.ts cancellableDelay() (~14 lines): new Promise + setTimeout + manual abort-listener add/remove, resolving true on elapse and false on abort, consumed once for the backoff wait.

## Source decision

Replace all three with import { setTimeout } from 'node:timers/promises': llm-retry: try { await setTimeout(delayMs, undefined, { signal }); / retry / } catch { / abort → fail / } --- with a signal, the promise rejects only with the abort error, and a pre-aborted signal rejects immediately; behavior is identical, including timer clearing on abort. The empty catch names the abort rejection per the repo's empty-catch rule. workflow-worker-thread: setTimeout(ms, undefined, { ref: false }) --- exact semantics including not holding the event loop open.

## Decision status

Rejected design. Do not implement it as written. Reuse its failure analysis, constraints, and reversal condition.

- Raw note: [rejected/simplification/2026-07-26-builtin-timer-promises-for-hand-rolled-sleeps.md](../02-notes/rejected/simplification/2026-07-26-builtin-timer-promises-for-hand-rolled-sleeps.md)
- Pinned source: [.agents/notes/rejected/simplification/2026-07-26-builtin-timer-promises-for-hand-rolled-sleeps.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/rejected/simplification/2026-07-26-builtin-timer-promises-for-hand-rolled-sleeps.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`packages/llm/llm-retry/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm-retry/src/index.ts) | package entry point | The source note names this file directly. Core file in the package named by the note: `packages/llm/llm-retry`. | `named-file, named-package-member, symbol-definition` |
| [`packages/terminal/terminal-bash/src/session.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/terminal/terminal-bash/src/session.ts) | runtime implementation | The source note names this file directly. | `named-file` |
| [`packages/workflow/workflow-worker-thread/src/host.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/workflow/workflow-worker-thread/src/host.ts) | runtime implementation | The source note names this file directly. Core file in the package named by the note: `packages/workflow/workflow-worker-thread`. | `named-file, named-package-member, symbol-definition` |
| [`packages/llm/llm-retry/src/types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm-retry/src/types.ts) | public types and contract | Core file in the package named by the note: `packages/llm/llm-retry`. | `named-package-member` |
| [`packages/lsp/lsp-stdio/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/lsp/lsp-stdio/src/index.ts) | package entry point | Core file in the package named by the note: `packages/lsp/lsp-stdio`. | `named-package-member` |
| [`packages/llm/llm-retry/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm-retry/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/llm/llm-retry`. | `named-package-member` |
| [`packages/lsp/lsp-stdio/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/lsp/lsp-stdio/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/lsp/lsp-stdio`. | `named-package-member` |
| [`packages/terminal/terminal-bash/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/terminal/terminal-bash/src/index.ts) | package entry point | Core file in the package named by the note: `packages/terminal/terminal-bash`. | `named-package-member` |
| [`packages/test-support/acp-snapshot/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/test-support/acp-snapshot/src/index.ts) | package entry point | Core file in the package named by the note: `packages/test-support/acp-snapshot`. | `named-package-member` |
| [`packages/terminal/terminal-bash/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/terminal/terminal-bash/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/terminal/terminal-bash`. | `named-package-member` |
| [`packages/test-support/llm-mock-server/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/test-support/llm-mock-server/src/index.ts) | package entry point | Core file in the package named by the note: `packages/test-support/llm-mock-server`. Defines `pause`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/test-support/acp-snapshot/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/test-support/acp-snapshot/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/test-support/acp-snapshot`. | `named-package-member` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `cancellableDelay` | `function` | [`packages/llm/llm-retry/src/index.ts:78`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm-retry/src/index.ts#L78) | `function cancellableDelay(delayMs: number, signal: AbortSignal): Promise<boolean> {` |
| `pause` | `function` | [`packages/test-support/llm-mock-server/src/index.ts:374`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/test-support/llm-mock-server/src/index.ts#L374) | `async function pause(milliseconds: number, response: ServerResponse): Promise<boolean> {` |
| `sleep` | `function` | [`packages/workflow/workflow-worker-thread/src/host.ts:617`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/workflow/workflow-worker-thread/src/host.ts#L617) | `function sleep(ms: number): Promise<void> {` |
| `delay` | `const` | [`vendor/timer/src/index.ts:33`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/timer/src/index.ts#L33) | `const delay = args[0] as number` |

### Tests and executable evidence

- [`packages/lsp/lsp-stdio/tests/host.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/lsp/lsp-stdio/tests/host.spec.ts) — A test under the owning area exercises or imports `dsh-lsp-stdio`.
- [`packages/lsp/lsp-stdio/tests/framing.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/lsp/lsp-stdio/tests/framing.spec.ts) — A test under the owning area exercises or imports `dsh-lsp-stdio`.
- [`packages/lsp/lsp-stdio/tests/provider.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/lsp/lsp-stdio/tests/provider.spec.ts) — A test under the owning area exercises or imports `dsh-lsp-stdio`.
- [`packages/lsp/lsp-stdio/tests/built-lib.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/lsp/lsp-stdio/tests/built-lib.e2e.ts) — A test under the owning area exercises or imports `dsh-lsp-stdio`.
- [`packages/lsp/lsp-stdio/tests/instance.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/lsp/lsp-stdio/tests/instance.spec.ts) — A test under the owning area exercises or imports `dsh-lsp-stdio`.
- [`packages/lsp/lsp-stdio/tests/fixture-server.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/lsp/lsp-stdio/tests/fixture-server.ts) — A test under the owning area exercises or imports `pause`. A test under the owning area exercises or imports `setTimeout`.
- [`packages/lsp/lsp-stdio/tests/lifecycle.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/lsp/lsp-stdio/tests/lifecycle.spec.ts) — A test under the owning area exercises or imports `dsh-lsp-stdio`. A test under the owning area exercises or imports `setTimeout`.
- [`packages/lsp/lsp-stdio/tests/translate.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/lsp/lsp-stdio/tests/translate.spec.ts) — A test under the owning area exercises or imports `dsh-lsp-stdio`.
- Source verification intent: None of the three packages defines a promise-wrapped setTimeout helper; all import from node:timers/promises. The llm-retry, workflow-worker-thread, and terminal-bash test suites pass unchanged (behavioral parity).

## How to read the implementation

1. Start with [`packages/llm/llm-retry/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm-retry/src/index.ts) because it has the strongest evidence link to the note.
2. Read public types and entry points before implementation details. Write down the contract in plain language.
3. Follow the named constructs into their callers and tests. Confirm which component can actually enforce the rule.
4. Reconstruct one failure path: cancellation, crash, retry, stale state, partial write, or incompatible provider input—whichever the note is protecting against.
5. Only then map the contract into the current Rust repository. Target paths must come from that repository, never from guesswork in this wiki.

## Rust harness port

**Likely integration point:** Rust provider-adapter boundary.

Preserve the behavior and ownership rule. Do not mechanically translate Cordis, TypeScript declaration merging, Node streams, or package topology into Rust.

Relevant standing rules:

- **`rust/append-log`** — Keep durable facts append-only and derive views from them. Separate the synchronous fact boundary from asynchronous persistence.
- **`rust/four-tool-surface`** — Preserve the four model tools: read, write, patch, and exec. Reuse internal capabilities without automatically exposing new model-facing tools.
- **`rust/process-discipline`** — Treat this as repository or release policy unless it changes a runtime contract. Do not add runtime machinery for a process-only lesson.
- **`rust/provider-adapter`** — Keep provider wire quirks inside adapters; the loop and durable event vocabulary remain provider-neutral.
- **`rust/tokio-cancellation`** — Thread one cancellation token through async work, close owned resources before reuse, and test cancellation at every await boundary.
- **`rust/trait-seam`** — Use a small trait at the capability boundary; keep policy and model-facing schemas outside the backend implementation.
- **Status gate** — Rejected design. Do not implement it as written. Reuse its failure analysis, constraints, and reversal condition.

### Smallest safe implementation slice

1. Express the semantic contract as Rust types and one owning boundary.
2. Add the smallest adapter or service needed by the existing loop; do not add a new model-facing tool unless the four-tool surface cannot express the job.
3. Record durable evidence before deriving summaries or UI state.
4. Add one happy-path test, one interruption test, and one replay/restart test where state survives a turn.
5. Keep the patch reversible until the source note's strongest failure case is proven in the Rust harness.

## Search handles

- Tags: `class/simplification`, `concern/boundary`, `concern/cancellation-timeout`, `concern/evidence`, `concern/lifecycle`, `concern/recovery`, `concern/simplification`, `domain/build-release`, `domain/llm`, `domain/protocols`, `domain/session-state`, `domain/shell-terminal`, `domain/storage`, `domain/testing`, `lifecycle/rejected`, `mechanism/projection`
- Aliases: `cancellableDelay`, `pause`, `sleep`, `delay`, `node:timers/promises`, `dsh-llm-mock-server`, `dsh-lsp-stdio`, `dsh-acp-snapshot`, `packages/llm/llm-retry/src/index.ts`, `new Promise`, `setTimeout`, `packages/workflow/workflow-worker-thread/src/host.ts`, `packages/terminal/terminal-bash/src/session.ts`, `import { setTimeout } from 'node:timers/promises`
- Regex: `(?i)(cancellableDelay|pause|sleep|delay|node:timers/promises|dsh\-llm\-mock\-server|dsh\-lsp\-stdio|dsh\-acp\-snapshot)`

```bash
rg -n --pcre2 "(?i)(cancellableDelay|pause|sleep|delay|node:timers/promises|dsh\\-llm\\-mock\\-server|dsh\\-lsp\\-stdio|dsh\\-acp\\-snapshot)" source-deepseek-harness
rg -l --fixed-strings "class/simplification" 03-agent-context
```

## Connected notes

- **`shares-code-with`** — [0542. Dependency swaps rejected by the 2026-07 NIH audit](0542-dependency-swaps-rejected-by-the-2026-07-nih-audit.md): Shares source implementation: `packages/llm/llm-retry/src/index.ts`, `packages/llm/llm-retry/src/types.ts`.
- **`shares-code-with`** — [0501. Scriptable LLM wire fault server](0501-scriptable-llm-wire-fault-server.md): Shares source implementation: `packages/llm/llm-retry/src/index.ts`, `packages/llm/llm-retry/src/invariant.ts`.
- **`shares-code-with`** — [0181. Per-provider request retry policies](0181-per-provider-request-retry-policies.md): Shares source implementation: `packages/llm/llm-retry/src/index.ts`, `packages/llm/llm-retry/src/invariant.ts`.
- **`shares-code-with`** — [0500. Keyless browser e2e lane for the web GUI](0500-keyless-browser-e2e-lane-for-the-web-gui.md): Shares source implementation: `packages/llm/llm-retry/src/index.ts`, `packages/llm/llm-retry/src/types.ts`.
- **`shares-code-with`** — [0072. Portable consumers over filesystem and subprocess execution worlds](0072-portable-consumers-over-filesystem-and-subprocess-execution-worlds.md): Shares source implementation: `packages/lsp/lsp-stdio/src/index.ts`, `packages/lsp/lsp-stdio/src/invariant.ts`.
- **`shares-code-with`** — [0037. Provider-routed LLM adapters and a generic pi-ai backend](0037-provider-routed-llm-adapters-and-a-generic-pi-ai-backend.md): Shares source implementation: `packages/llm/llm-retry/src/index.ts`, `packages/llm/llm-retry/src/types.ts`.
- **`shares-code-with`** — [0016. Bounded recovery for transient LLM request failures](0016-bounded-recovery-for-transient-llm-request-failures.md): Shares source implementation: `packages/llm/llm-retry/src/index.ts`, `packages/llm/llm-retry/src/types.ts`.
- **`shares-code-with`** — [0399. Serial cross-platform CI reference](0399-serial-cross-platform-ci-reference.md): Shares source implementation: `packages/terminal/terminal-bash/src/index.ts`, `packages/terminal/terminal-bash/src/invariant.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0541-use-node-timers-promises-for-hand-rolled-cancellable-sleeps.md`.
