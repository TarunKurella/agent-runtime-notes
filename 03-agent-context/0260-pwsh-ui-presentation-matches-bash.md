---
id: "dsh-note-0260"
title: "pwsh UI presentation matches bash"
status: "implemented"
class: "feature"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/feature/2026-08-05-pwsh-ui-bash-parity.md"
implementation_evidence: "high"
target_anchor: "read/write/patch filesystem capability"
tags:
  - "class/feature"
  - "concern/boundary"
  - "concern/cancellation-timeout"
  - "concern/discovery-routing"
  - "concern/evidence"
  - "concern/human-control"
  - "concern/ownership"
  - "concern/recovery"
  - "domain/filesystem"
  - "domain/llm"
  - "domain/session-state"
  - "domain/shell-terminal"
  - "domain/testing"
  - "domain/tools"
  - "domain/ui-interaction"
  - "domain/web-retrieval"
  - "lifecycle/implemented"
  - "mechanism/event-sourcing"
  - "mechanism/policy"
  - "mechanism/projection"
  - "mechanism/registry"
aliases:
  - "exitCode"
  - "classifyTool"
  - "terminal"
  - "ParsedExitStatus"
  - "parseExitStatus"
  - "signal"
  - "ShellRunResult"
  - "presentResult"
  - "dsh-tool-pwsh"
  - "dsh-tool-bash"
  - "isError"
  - "@deepseek-ai/dsh-shell"
  - "render.ts"
  - "[exit code: N]"
search_regex: "(?i)(exitCode|classifyTool|terminal|ParsedExitStatus|parseExitStatus|signal|ShellRunResult|presentResult)"
---

# 0260. pwsh UI presentation matches bash — implementation context

## Open this when

The pwsh tool bash parity decision made dsh-tool-pwsh behaviorally interchangeable with dsh-tool-bash for execution, markers, and background jobs, but explicitly deferred the human-visible half: a completed pwsh foreground call presented as a generic console-fenced card while the bash tool's completed call presented as a terminal card with a parsed exit-status pill. The roadmap that owned this gap (Windows defaults to pwsh) named "pwsh TUI/GUI rendering" as stage 2, but the TUI package was removed, leaving the Web surface as the only UI the gap affects.

## Source decision

dsh-tool-pwsh's presentResult now mirrors dsh-tool-bash's call-for-call: a completed foreground result is a terminal card whose output body is the marker-free rendered text and whose exit-status pill is the parsed exitCode/signal; background acknowledgements and isError results stay generic console-fenced cards; non-single-text-block results stay untouched (undefined).

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/feature/2026-08-05-pwsh-ui-bash-parity.md](../02-notes/implemented/feature/2026-08-05-pwsh-ui-bash-parity.md)
- Pinned source: [.agents/notes/implemented/feature/2026-08-05-pwsh-ui-bash-parity.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/feature/2026-08-05-pwsh-ui-bash-parity.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`packages/shell/shell/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/shell/shell/src/index.ts) | package entry point | Core file in the package named by the note: `packages/shell/shell`. | `named-package-member` |
| [`packages/shell/shell/src/types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/shell/shell/src/types.ts) | public types and contract | Core file in the package named by the note: `packages/shell/shell`. Defines `ShellRunResult`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/shell/shell/src/render.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/shell/shell/src/render.ts) | runtime implementation | Core file in the package named by the note: `packages/shell/shell`. Defines `signal`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/shell/shell/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/shell/shell/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/shell/shell`. | `named-package-member` |
| [`packages/shell/tool-bash/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/shell/tool-bash/src/index.ts) | package entry point | Core file in the package named by the note: `packages/shell/tool-bash`. | `named-package-member` |
| [`packages/shell/tool-pwsh/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/shell/tool-pwsh/src/index.ts) | package entry point | Core file in the package named by the note: `packages/shell/tool-pwsh`. | `named-package-member` |
| [`packages/terminal/terminal/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/terminal/terminal/src/index.ts) | package entry point | Core file in the package named by the note: `packages/terminal/terminal`. | `named-package-member` |
| [`packages/terminal/terminal/src/types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/terminal/terminal/src/types.ts) | public types and contract | Core file in the package named by the note: `packages/terminal/terminal`. | `named-package-member` |
| [`packages/shell/tool-bash/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/shell/tool-bash/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/shell/tool-bash`. | `named-package-member` |
| [`packages/shell/tool-pwsh/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/shell/tool-pwsh/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/shell/tool-pwsh`. | `named-package-member` |
| [`packages/terminal/terminal/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/terminal/terminal/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/terminal/terminal`. | `named-package-member` |
| [`packages/client/ui-conversation/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/src/index.ts) | package entry point | Core file in the package named by the note: `packages/client/ui-conversation`. | `named-package-member` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `exitCode` | `const` | [`apps/cli/src/plugin.ts:142`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/cli/src/plugin.ts#L142) | `const exitCode = result.status ?? 1` |
| `classifyTool` | `function` | [`packages/client/ui-tool/src/client/tool/models/tool-call-model.ts:76`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-tool/src/client/tool/models/tool-call-model.ts#L76) | `export function classifyTool(toolName: string): ToolRowVariant {` |
| `terminal` | `const` | [`packages/jobs/jobs/src/invariant.ts:30`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/jobs/jobs/src/invariant.ts#L30) | `const terminal = TERMINAL_STATUSES.has(snapshot.status)` |
| `ParsedExitStatus` | `type` | [`packages/shell/shell/src/render.ts:12`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/shell/shell/src/render.ts#L12) | `export type ParsedExitStatus =` |
| `parseExitStatus` | `function` | [`packages/shell/shell/src/render.ts:36`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/shell/shell/src/render.ts#L36) | `export function parseExitStatus(text: string): ParsedExitStatus {` |
| `signal` | `const` | [`packages/shell/shell/src/render.ts:37`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/shell/shell/src/render.ts#L37) | `const signal = /\n\[killed by signal: ([^\]\n]+)\]$/.exec(text)` |
| `ShellRunResult` | `interface` | [`packages/shell/shell/src/types.ts:113`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/shell/shell/src/types.ts#L113) | `export interface ShellRunResult {` |
| `presentResult` | `function` | [`packages/workflow/tool-ralph/src/index.ts:398`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/workflow/tool-ralph/src/index.ts#L398) | `function presentResult(args: RalphCallArgs, result: { content: ContentBlock[]; isError: boolean }): ToolResultView {` |

### Tests and executable evidence

- [`apps/web/tests/pwsh-terminal.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/pwsh-terminal.e2e.ts) — The source note names this file directly.
- [`apps/cli/tests/args.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/cli/tests/args.spec.ts) — A test under the owning area exercises or imports `exitCode`.
- [`apps/cli/tests/built-bin.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/cli/tests/built-bin.e2e.ts) — A test under the owning area exercises or imports `exitCode`.
- [`apps/cli/tests/process-shutdown.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/cli/tests/process-shutdown.spec.ts) — A test under the owning area exercises or imports `exitCode`.
- [`apps/cli/tests/headless-shutdown.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/cli/tests/headless-shutdown.e2e.ts) — A test under the owning area exercises or imports `exitCode`.
- [`packages/shell/shell/tests/render.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/shell/shell/tests/render.spec.ts) — A test under the owning area exercises or imports `dsh-tool-pwsh`. A test under the owning area exercises or imports `parseExitStatus`.
- [`packages/shell/shell/tests/service.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/shell/shell/tests/service.spec.ts) — A test under the owning area exercises or imports `ShellRunResult`.
- [`apps/cli/tests/source-launch.compat.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/cli/tests/source-launch.compat.spec.ts) — A test under the owning area exercises or imports `exitCode`.

## How to read the implementation

1. Start with [`packages/shell/shell/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/shell/shell/src/index.ts) because it has the strongest evidence link to the note.
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
- **`rust/provider-adapter`** — Keep provider wire quirks inside adapters; the loop and durable event vocabulary remain provider-neutral.
- **`rust/tokio-cancellation`** — Thread one cancellation token through async work, close owned resources before reuse, and test cancellation at every await boundary.
- **`rust/trait-seam`** — Use a small trait at the capability boundary; keep policy and model-facing schemas outside the backend implementation.

### Smallest safe implementation slice

1. Express the semantic contract as Rust types and one owning boundary.
2. Add the smallest adapter or service needed by the existing loop; do not add a new model-facing tool unless the four-tool surface cannot express the job.
3. Record durable evidence before deriving summaries or UI state.
4. Add one happy-path test, one interruption test, and one replay/restart test where state survives a turn.
5. Keep the patch reversible until the source note's strongest failure case is proven in the Rust harness.

## Search handles

- Tags: `class/feature`, `concern/boundary`, `concern/cancellation-timeout`, `concern/discovery-routing`, `concern/evidence`, `concern/human-control`, `concern/ownership`, `concern/recovery`, `domain/filesystem`, `domain/llm`, `domain/session-state`, `domain/shell-terminal`, `domain/testing`, `domain/tools`, `domain/ui-interaction`, `domain/web-retrieval`, `lifecycle/implemented`, `mechanism/event-sourcing`, `mechanism/policy`, `mechanism/projection`, `mechanism/registry`
- Aliases: `exitCode`, `classifyTool`, `terminal`, `ParsedExitStatus`, `parseExitStatus`, `signal`, `ShellRunResult`, `presentResult`, `dsh-tool-pwsh`, `dsh-tool-bash`, `isError`, `@deepseek-ai/dsh-shell`, `render.ts`, `[exit code: N]`
- Regex: `(?i)(exitCode|classifyTool|terminal|ParsedExitStatus|parseExitStatus|signal|ShellRunResult|presentResult)`

```bash
rg -n --pcre2 "(?i)(exitCode|classifyTool|terminal|ParsedExitStatus|parseExitStatus|signal|ShellRunResult|presentResult)" source-deepseek-harness
rg -l --fixed-strings "class/feature" 03-agent-context
```

## Connected notes

- **`source-link`** — [0241. Windows defaults to pwsh](0241-windows-defaults-to-pwsh.md): The source note links to this decision directly.
- **`source-link`** — [0242. pwsh tool bash parity](0242-pwsh-tool-bash-parity.md): The source note links to this decision directly.
- **`shares-code-with`** — [0240. PowerShell executor and pwsh tool](0240-powershell-executor-and-pwsh-tool.md): Shares source implementation: `packages/shell/shell/src/index.ts`, `packages/shell/shell/src/invariant.ts`.
- **`shares-code-with`** — [0011. Agent lifecycle and ownership contracts](0011-agent-lifecycle-and-ownership-contracts.md): Shares source implementation: `packages/shell/shell/src/index.ts`, `packages/shell/shell/src/invariant.ts`.
- **`shares-code-with`** — [0007. Capability seams --- Service Definition / Service Provider / Consumer roles](0007-capability-seams-service-definition-service-provider-consumer-roles.md): Shares source implementation: `packages/shell/shell/src/index.ts`, `packages/shell/shell/src/invariant.ts`.
- **`shares-code-with`** — [0030. Tool-call timeout policy as a plugin](0030-tool-call-timeout-policy-as-a-plugin.md): Shares source implementation: `packages/shell/shell/src/index.ts`, `packages/shell/shell/src/render.ts`.
- **`shares-code-with`** — [0014. Branded IDs everywhere they belong](0014-branded-ids-everywhere-they-belong.md): Shares source implementation: `packages/shell/shell/src/index.ts`, `packages/shell/shell/src/types.ts`.
- **`shares-code-with`** — [0193. tmux-location context](0193-tmux-location-context.md): Shares source implementation: `packages/shell/shell/src/index.ts`, `packages/shell/shell/src/invariant.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0260-pwsh-ui-presentation-matches-bash.md`.
