---
id: "dsh-note-0068"
title: "The dispose ladder belongs to its consumer, not the subprocess seam"
status: "implemented"
class: "architecture"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/architecture/2026-07-27-dispose-ladder-to-consumer.md"
implementation_evidence: "medium"
target_anchor: "Rust provider-adapter boundary"
tags:
  - "class/architecture"
  - "concern/boundary"
  - "concern/cancellation-timeout"
  - "concern/lifecycle"
  - "concern/ownership"
  - "domain/configuration"
  - "domain/extensions"
  - "domain/llm"
  - "domain/multi-agent"
  - "domain/protocols"
  - "domain/shell-terminal"
  - "lifecycle/implemented"
  - "mechanism/policy"
aliases:
  - "stdin"
  - "disposeAcpChild"
  - "kill"
  - "terminate"
  - "waitForExit"
  - "SubprocessHandle.dispose"
  - "SubprocessDisposeGraces"
  - "dsh-timeout"
  - "dsh-subagent-acp"
  - "eofGraceMs"
  - "dsh-subagent-subprocess"
  - "The dispose ladder belongs to its consumer, not the subprocess seam"
  - "architecture"
  - "boundary"
search_regex: "(?i)(stdin|disposeAcpChild|kill|terminate|waitForExit|SubprocessHandle\\.dispose|SubprocessDisposeGraces|dsh\\-timeout)"
---

# 0068. The dispose ladder belongs to its consumer, not the subprocess seam — implementation context

## Open this when

SubprocessHandle.dispose(graces) and SubprocessDisposeGraces put a full teardown policy --- stdin-EOF wait, then SIGTERM, then SIGKILL, each tier bounded by a caller-supplied window --- on a seam whose other verbs are single mechanisms. Only one consumer ever called it (the ACP subagent backend); bash rides terminate() and service teardown, and the LSP host runs its own protocol-first shutdown. Every future backend nonetheless had to implement the ladder to satisfy the interface, and the implementation carried a dsh-timeout dependency solely for the ladder's tier bounds.

## Source decision

The ladder moves to its one consumer. dsh-subagent-acp owns disposeAcpChild(child, eofGraceMs), built entirely on the seam's public verbs: close stdin, bound a waitForExit on eofGraceMs, then call terminate(), whose SIGTERM→spec-grace→SIGKILL escalation already owns the signal timer, and await an unbounded waitForExit() for the subprocess owner's whole-tree exit proof.

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/architecture/2026-07-27-dispose-ladder-to-consumer.md](../02-notes/implemented/architecture/2026-07-27-dispose-ladder-to-consumer.md)
- Pinned source: [.agents/notes/implemented/architecture/2026-07-27-dispose-ladder-to-consumer.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/architecture/2026-07-27-dispose-ladder-to-consumer.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`packages/util/timeout/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/util/timeout/src/index.ts) | package entry point | Core file in the package named by the note: `packages/util/timeout`. | `named-package-member` |
| [`packages/util/timeout/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/util/timeout/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/util/timeout`. | `named-package-member` |
| [`packages/subagent/subagent-acp/src/run.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/subagent/subagent-acp/src/run.ts) | runtime implementation | Core file in the package named by the note: `packages/subagent/subagent-acp`. Defines `disposeAcpChild`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/subagent/subagent-acp/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/subagent/subagent-acp/src/index.ts) | package entry point | Core file in the package named by the note: `packages/subagent/subagent-acp`. | `named-package-member` |
| [`packages/subagent/subagent-acp/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/subagent/subagent-acp/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/subagent/subagent-acp`. | `named-package-member` |
| [`packages/util/timeout`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/packages/util/timeout) | package or module directory | The note names this package or capability. | `named-package` |
| [`packages/subagent/subagent-acp`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/packages/subagent/subagent-acp) | package or module directory | The note names this package or capability. | `named-package` |
| [`packages/hooks/hook-protocol/src/runner.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/hooks/hook-protocol/src/runner.ts) | runtime implementation | Defines `stdin`, a construct named by the note. | `symbol-definition` |
| [`packages/subprocess/subprocess-local/src/spawn.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/subprocess/subprocess-local/src/spawn.ts) | runtime implementation | Defines `terminate`, a construct named by the note. Defines `waitForExit`, a construct named by the note. | `symbol-definition` |
| [`packages/util/timeout/README.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/util/timeout/README.md) | package contract and examples | Core file in the package named by the note: `packages/util/timeout`. | `named-package-member` |
| [`packages/util/timeout/package.json`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/util/timeout/package.json) | composition and configuration | Core file in the package named by the note: `packages/util/timeout`. | `named-package-member` |
| [`packages/subagent/subagent-acp/README.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/subagent/subagent-acp/README.md) | package contract and examples | Core file in the package named by the note: `packages/subagent/subagent-acp`. | `named-package-member` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `stdin` | `const` | [`packages/hooks/hook-protocol/src/runner.ts:75`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/hooks/hook-protocol/src/runner.ts#L75) | `const stdin = JSON.stringify(options.payload) + (options.trailingNewline ? '\n' : '')` |
| `disposeAcpChild` | `function` | [`packages/subagent/subagent-acp/src/run.ts:114`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/subagent/subagent-acp/src/run.ts#L114) | `export async function disposeAcpChild(child: SubprocessHandle, eofGraceMs: number): Promise<void> {` |
| `kill` | `const` | [`packages/subprocess/subprocess-local/src/spawn.ts:432`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/subprocess/subprocess-local/src/spawn.ts#L432) | `const kill = (sig: NodeJS.Signals): void => {` |
| `terminate` | `const` | [`packages/subprocess/subprocess-local/src/spawn.ts:439`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/subprocess/subprocess-local/src/spawn.ts#L439) | `const terminate = (): void => {` |
| `waitForExit` | `const` | [`packages/subprocess/subprocess-local/src/spawn.ts:507`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/subprocess/subprocess-local/src/spawn.ts#L507) | `const waitForExit = async (signal?: AbortSignal): Promise<boolean> => {` |

### Tests and executable evidence

- [`packages/subagent/subagent-acp/tests/mock-acp-server.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/subagent/subagent-acp/tests/mock-acp-server.ts) — A test under the owning area exercises or imports `dsh-subagent-acp`.
- [`packages/subprocess/subprocess-local/tests/spawn.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/subprocess/subprocess-local/tests/spawn.spec.ts) — A test under the owning area exercises or imports `terminate`. A test under the owning area exercises or imports `waitForExit`.
- [`packages/subprocess/subprocess-local/tests/local.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/subprocess/subprocess-local/tests/local.spec.ts) — A test under the owning area exercises or imports `terminate`. A test under the owning area exercises or imports `waitForExit`.
- [`packages/subagent/subagent-acp/tests/subagent-acp.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/subagent/subagent-acp/tests/subagent-acp.spec.ts) — A test under the owning area exercises or imports `dsh-subagent-acp`. A test under the owning area exercises or imports `disposeAcpChild`.
- [`packages/subprocess/subprocess-local/tests/terminal.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/subprocess/subprocess-local/tests/terminal.spec.ts) — A test under the owning area exercises or imports `terminate`.

## How to read the implementation

1. Start with [`packages/util/timeout/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/util/timeout/src/index.ts) because it has the strongest evidence link to the note.
2. Read public types and entry points before implementation details. Write down the contract in plain language.
3. Follow the named constructs into their callers and tests. Confirm which component can actually enforce the rule.
4. Reconstruct one failure path: cancellation, crash, retry, stale state, partial write, or incompatible provider input—whichever the note is protecting against.
5. Only then map the contract into the current Rust repository. Target paths must come from that repository, never from guesswork in this wiki.

## Rust harness port

**Likely integration point:** Rust provider-adapter boundary.

Preserve the behavior and ownership rule. Do not mechanically translate Cordis, TypeScript declaration merging, Node streams, or package topology into Rust.

Relevant standing rules:

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

- Tags: `class/architecture`, `concern/boundary`, `concern/cancellation-timeout`, `concern/lifecycle`, `concern/ownership`, `domain/configuration`, `domain/extensions`, `domain/llm`, `domain/multi-agent`, `domain/protocols`, `domain/shell-terminal`, `lifecycle/implemented`, `mechanism/policy`
- Aliases: `stdin`, `disposeAcpChild`, `kill`, `terminate`, `waitForExit`, `SubprocessHandle.dispose`, `SubprocessDisposeGraces`, `dsh-timeout`, `dsh-subagent-acp`, `eofGraceMs`, `dsh-subagent-subprocess`, `The dispose ladder belongs to its consumer, not the subprocess seam`, `architecture`, `boundary`
- Regex: `(?i)(stdin|disposeAcpChild|kill|terminate|waitForExit|SubprocessHandle\.dispose|SubprocessDisposeGraces|dsh\-timeout)`

```bash
rg -n --pcre2 "(?i)(stdin|disposeAcpChild|kill|terminate|waitForExit|SubprocessHandle\\.dispose|SubprocessDisposeGraces|dsh\\-timeout)" source-deepseek-harness
rg -l --fixed-strings "class/architecture" 03-agent-context
```

## Connected notes

- **`shares-code-with`** — [0042. Explicit turn cancellation capability](0042-explicit-turn-cancellation-capability.md): Shares source implementation: `packages/util/timeout`, `packages/util/timeout/src/index.ts`.
- **`shares-code-with`** — [0459. ACP as an automation-only protocol](0459-acp-as-an-automation-only-protocol.md): Shares source implementation: `packages/subagent/subagent-acp`, `packages/subagent/subagent-acp/src/index.ts`.
- **`shares-code-with`** — [0285. Delegated subagents run with approvals pinned to `'never'`](0285-delegated-subagents-run-with-approvals-pinned-to-never.md): Shares source implementation: `packages/subagent/subagent-acp`, `packages/subagent/subagent-acp/src/index.ts`.
- **`shares-code-with`** — [0135. ACP subagent backend (out-of-process delegation)](0135-acp-subagent-backend-out-of-process-delegation.md): Shares source implementation: `packages/subagent/subagent-acp/src/index.ts`, `packages/subagent/subagent-acp/src/run.ts`.
- **`shares-code-with`** — [0028. A shared timeout/deadline primitive, with hard-kill left to each capability](0028-a-shared-timeout-deadline-primitive-with-hard-kill-left-to-each-capabili.md): Shares source implementation: `packages/subprocess/subprocess-local/src/spawn.ts`, `packages/util/timeout/src/index.ts`.
- **`shares-code-with`** — [0029. Tool result retention library](0029-tool-result-retention-library.md): Shares source implementation: `packages/util/timeout/src/index.ts`, `packages/util/timeout/src/invariant.ts`.
- **`shares-code-with`** — [0306. Classify pi-ai transport truncations from flattened message text](0306-classify-pi-ai-transport-truncations-from-flattened-message-text.md): Shares source implementation: `packages/util/timeout/src/index.ts`, `packages/util/timeout/src/invariant.ts`.
- **`shares-code-with`** — [0142. Dynamic workflows --- a script-driven multi-agent orchestration seam](0142-dynamic-workflows-a-script-driven-multi-agent-orchestration-seam.md): Shares source implementation: `packages/util/timeout/src/index.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0068-the-dispose-ladder-belongs-to-its-consumer-not-the-subprocess-seam.md`.
