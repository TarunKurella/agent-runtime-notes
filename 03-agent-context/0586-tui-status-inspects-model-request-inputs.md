---
id: "dsh-note-0586"
title: "TUI status inspects model request inputs"
status: "archived"
class: "feature"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/archived/feature/2026-07-23-tui-status-prompt-tools.md"
implementation_evidence: "medium"
target_anchor: "Rust provider-adapter boundary"
tags:
  - "class/feature"
  - "concern/cancellation-timeout"
  - "concern/discovery-routing"
  - "concern/schema-types"
  - "domain/agent-loop"
  - "domain/build-release"
  - "domain/context"
  - "domain/llm"
  - "domain/session-state"
  - "domain/shell-terminal"
  - "domain/storage"
  - "domain/testing"
  - "domain/tools"
  - "domain/ui-interaction"
  - "lifecycle/archived"
  - "mechanism/registry"
aliases:
  - "systemPrompt"
  - "/status"
  - "ctx.systemPrompt"
  - "TUI status inspects model request inputs"
  - "feature"
  - "cancellation timeout"
  - "discovery routing"
  - "schema types"
  - "agent loop"
  - "build release"
  - "context"
  - "llm"
  - "session state"
  - "shell terminal"
search_regex: "(?i)(systemPrompt|/status|ctx\\.systemPrompt|TUI[- ]status[- ]inspects[- ]model[- ]request[- ]inputs|feature|cancellation[- ]timeout|discovery[- ]routing|schema[- ]types)"
---

# 0586. TUI status inspects model request inputs — implementation context

## Open this when

Session counters describe activity but do not reveal the instructions and capabilities that the next model request receives. Diagnosing scoped prompt contributions and tool restrictions otherwise requires leaving the TUI or inferring configuration from files.

## Source decision

/status assembles the current agent's system prompt through ctx.systemPrompt and renders it with the same renderer used by the agent loop. After the bordered diagnostics card, separate unbordered System prompt and Registered tools sections show the rendered prompt and the assembly's ordered tool names, which are the schemas exposed to the model for that agent and presentation mode. Assembly uses the command's cancellation signal and current agent scope, so scoped sections, variables, tool restrictions, and assembly listeners match a request made at that point.

## Decision status

Historical design. Use it to understand an older pressure or failure; locate the replacing decision before implementing anything.

- Raw note: [archived/feature/2026-07-23-tui-status-prompt-tools.md](../02-notes/archived/feature/2026-07-23-tui-status-prompt-tools.md)
- Pinned source: [.agents/notes/archived/feature/2026-07-23-tui-status-prompt-tools.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/archived/feature/2026-07-23-tui-status-prompt-tools.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`packages/boot/app-boot/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/boot/app-boot/src/index.ts) | package entry point | Defines `systemPrompt`, a construct named by the note. | `symbol-definition` |
| [`packages/session/session-title-llm/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/session/session-title-llm/src/index.ts) | package entry point | Defines `systemPrompt`, a construct named by the note. | `symbol-definition` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `systemPrompt` | `const` | [`packages/boot/app-boot/src/index.ts:822`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/boot/app-boot/src/index.ts#L822) | `const systemPrompt = ctx.get('systemPrompt')` |
| `systemPrompt` | `function` | [`packages/session/session-title-llm/src/index.ts:186`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/session/session-title-llm/src/index.ts#L186) | `function systemPrompt(config: ResolvedSessionTitleLlmConfig): string {` |

### Tests and executable evidence

- No test file was tied to this note with enough confidence. Read the note's verification section and search the owning package's `tests/` directory.
- Source verification intent: Package behavior tests pin scoped assembly output, ordered tool names, empty labels, and terminal-control escaping. The package semantic snapshots exercise /status at normal and narrow widths; a deployment shipping the TUI owns its assembled process acceptance.

## How to read the implementation

1. Start with [`packages/boot/app-boot/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/boot/app-boot/src/index.ts) because it has the strongest evidence link to the note.
2. Read public types and entry points before implementation details. Write down the contract in plain language.
3. Follow the named constructs into their callers and tests. Confirm which component can actually enforce the rule.
4. Reconstruct one failure path: cancellation, crash, retry, stale state, partial write, or incompatible provider input—whichever the note is protecting against.
5. Only then map the contract into the current Rust repository. Target paths must come from that repository, never from guesswork in this wiki.

## Rust harness port

**Likely integration point:** Rust provider-adapter boundary.

Preserve the behavior and ownership rule. Do not mechanically translate Cordis, TypeScript declaration merging, Node streams, or package topology into Rust.

Relevant standing rules:

- **`rust/append-log`** — Keep durable facts append-only and derive views from them. Separate the synchronous fact boundary from asynchronous persistence.
- **`rust/context-evidence`** — Compaction may shrink presentation, but it must not erase the evidence ledger, file activity, or durable session facts.
- **`rust/four-tool-surface`** — Preserve the four model tools: read, write, patch, and exec. Reuse internal capabilities without automatically exposing new model-facing tools.
- **`rust/newtype-ids`** — Represent identity domains with Rust newtypes; do not pass bare strings across session, call, turn, step, and agent boundaries.
- **`rust/process-discipline`** — Treat this as repository or release policy unless it changes a runtime contract. Do not add runtime machinery for a process-only lesson.
- **`rust/provider-adapter`** — Keep provider wire quirks inside adapters; the loop and durable event vocabulary remain provider-neutral.
- **`rust/serde-validation`** — Parse untrusted input into strict serde types and validate semantic constraints before executing side effects.
- **`rust/tagged-enums`** — Use serde-tagged enums for a closed wire vocabulary. Add an explicit extension envelope only where runtime extension is truly required.
- **`rust/tokio-cancellation`** — Thread one cancellation token through async work, close owned resources before reuse, and test cancellation at every await boundary.
- **Status gate** — Historical design. Use it to understand an older pressure or failure; locate the replacing decision before implementing anything.

### Smallest safe implementation slice

1. Express the semantic contract as Rust types and one owning boundary.
2. Add the smallest adapter or service needed by the existing loop; do not add a new model-facing tool unless the four-tool surface cannot express the job.
3. Record durable evidence before deriving summaries or UI state.
4. Add one happy-path test, one interruption test, and one replay/restart test where state survives a turn.
5. Keep the patch reversible until the source note's strongest failure case is proven in the Rust harness.

## Search handles

- Tags: `class/feature`, `concern/cancellation-timeout`, `concern/discovery-routing`, `concern/schema-types`, `domain/agent-loop`, `domain/build-release`, `domain/context`, `domain/llm`, `domain/session-state`, `domain/shell-terminal`, `domain/storage`, `domain/testing`, `domain/tools`, `domain/ui-interaction`, `lifecycle/archived`, `mechanism/registry`
- Aliases: `systemPrompt`, `/status`, `ctx.systemPrompt`, `TUI status inspects model request inputs`, `feature`, `cancellation timeout`, `discovery routing`, `schema types`, `agent loop`, `build release`, `context`, `llm`, `session state`, `shell terminal`
- Regex: `(?i)(systemPrompt|/status|ctx\.systemPrompt|TUI[- ]status[- ]inspects[- ]model[- ]request[- ]inputs|feature|cancellation[- ]timeout|discovery[- ]routing|schema[- ]types)`

```bash
rg -n --pcre2 "(?i)(systemPrompt|/status|ctx\\.systemPrompt|TUI[- ]status[- ]inspects[- ]model[- ]request[- ]inputs|feature|cancellation[- ]timeout|discovery[- ]routing|schema[- ]types)" source-deepseek-harness
rg -l --fixed-strings "class/feature" 03-agent-context
```

## Connected notes

- **`shares-code-with`** — [0172. Log-backed session titles](0172-log-backed-session-titles.md): Shares source implementation: `packages/session/session-title-llm/src/index.ts`.
- **`shares-code-with`** — [0145. Explicit model-facing tool order](0145-explicit-model-facing-tool-order.md): Shares source implementation: `packages/boot/app-boot/src/index.ts`.
- **`shares-code-with`** — [0277. User-explicit skill invocation at the pre-step gesture boundary](0277-user-explicit-skill-invocation-at-the-pre-step-gesture-boundary.md): Shares source implementation: `packages/boot/app-boot/src/index.ts`.
- **`shares-code-with`** — [0284. A minimal read_image tool over existing seams](0284-a-minimal-read-image-tool-over-existing-seams.md): Shares source implementation: `packages/boot/app-boot/src/index.ts`.
- **`shares-code-with`** — [0075. Regroup packages/ by measured clustering](0075-regroup-packages-by-measured-clustering.md): Shares source implementation: `packages/boot/app-boot/src/index.ts`.
- **`same-design-pressure`** — [0131. Code Mode --- the model writes TypeScript against the tool registry](0131-code-mode-the-model-writes-typescript-against-the-tool-registry.md): Shares design concerns: `concern/cancellation-timeout`, `concern/discovery-routing`, `concern/schema-types`.
- **`shares-code-with`** — [0359. Pre-Plugin Theme Bootstrap](0359-pre-plugin-theme-bootstrap.md): Shares source implementation: `packages/session/session-title-llm/src/index.ts`.
- **`shares-code-with`** — [0263. The continuable child return channel is an obligation](0263-the-continuable-child-return-channel-is-an-obligation.md): Shares source implementation: `packages/boot/app-boot/src/index.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0586-tui-status-inspects-model-request-inputs.md`.
