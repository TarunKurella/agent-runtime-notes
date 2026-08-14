---
id: "dsh-note-0594"
title: "Fixed `Tool / <name>` header for tool-call cards"
status: "archived"
class: "feature"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/archived/feature/2026-07-27-tui-tool-card-header.md"
implementation_evidence: "medium"
target_anchor: "Rust provider-adapter boundary"
tags:
  - "class/feature"
  - "concern/boundary"
  - "concern/evidence"
  - "concern/lifecycle"
  - "concern/ownership"
  - "concern/recovery"
  - "domain/build-release"
  - "domain/llm"
  - "domain/protocols"
  - "domain/session-state"
  - "domain/shell-terminal"
  - "domain/testing"
  - "domain/tools"
  - "domain/ui-interaction"
  - "lifecycle/archived"
  - "mechanism/policy"
  - "mechanism/projection"
aliases:
  - "presentCall"
  - "presentResult"
  - "title"
  - "Tool / <name>"
  - "Read src/index.ts"
  - "{ring} Tool / <name>"
  - ". The ring marker is"
  - "/ <desc>"
  - "● Tool / bash / Run the coverage gate"
  - "ToolCardComponent"
  - "packages/ui/tui/src/components/transcript.ts"
  - "presentation.ts"
  - "Tool / read / Read src/index.ts"
  - "[exit N]"
search_regex: "(?i)(presentCall|presentResult|title|Tool[- ]/[- ]<name>|Read[- ]src/index\\.ts|\\{ring\\}[- ]Tool[- ]/[- ]<name>|\\.[- ]The[- ]ring[- ]marker[- ]is|/[- ]<desc>)"
---

# 0594. Fixed `Tool / <name>` header for tool-call cards — implementation context

## Open this when

The TUI rendered each tool call as {glyph} {title}, where title was the presenter's fused verb-plus-detail string (Read src/index.ts (1200-1360), Edit files, or a bash card's model description), bold and underlined in the status color. One flat slot carried the tool identity, the target, and the status at once, and the styling mixed bold, underline, and color inconsistently --- the header read as noise, and which tool ran was not visually separable from what it operated on.

## Source decision

The header is a fixed {ring} Tool / frame in a single flat status color --- no bold, no underline, no dim --- so one color reads consistently across the whole row. Tool is a literal constant; is the raw tool name. The separator is ASCII /. The ring marker is ○ while the call is pending and ● once it settles; the header color (warning pending / success ok / error) distinguishes pending from ok from error, so the same filled ring serves both settled states.

## Decision status

Historical design. Use it to understand an older pressure or failure; locate the replacing decision before implementing anything.

- Raw note: [archived/feature/2026-07-27-tui-tool-card-header.md](../02-notes/archived/feature/2026-07-27-tui-tool-card-header.md)
- Pinned source: [.agents/notes/archived/feature/2026-07-27-tui-tool-card-header.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/archived/feature/2026-07-27-tui-tool-card-header.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`packages/client/web/src/app.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/web/src/app.tsx) | runtime implementation | Defines `title`, a construct named by the note. | `symbol-definition` |
| [`packages/host/apiproxy/src/api-proxy.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/src/api-proxy.ts) | runtime implementation | Defines `title`, a construct named by the note. | `symbol-definition` |
| [`packages/workflow/tool-ralph/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/workflow/tool-ralph/src/index.ts) | package entry point | Defines `presentCall`, a construct named by the note. Defines `presentResult`, a construct named by the note. | `symbol-definition` |
| [`packages/session/session-title/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/session/session-title/src/index.ts) | package entry point | Defines `title`, a construct named by the note. | `symbol-definition` |
| [`packages/client/connection/src/client/fixture.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/connection/src/client/fixture.ts) | runtime implementation | Defines `presentCall`, a construct named by the note. Defines `presentResult`, a construct named by the note. | `symbol-definition` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `presentCall` | `function` | [`packages/client/connection/src/client/fixture.ts:607`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/connection/src/client/fixture.ts#L607) | `function presentCall(name: string, argsRaw: string): ToolCallView \| undefined {` |
| `presentResult` | `function` | [`packages/client/connection/src/client/fixture.ts:672`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/connection/src/client/fixture.ts#L672) | `function presentResult(name: string, argsRaw: string, resultText: string): ToolResultView \| undefined {` |
| `title` | `const` | [`packages/client/web/src/app.tsx:32`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/web/src/app.tsx#L32) | `const title = useSessions((state) => {` |
| `title` | `const` | [`packages/host/apiproxy/src/api-proxy.ts:2831`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/src/api-proxy.ts#L2831) | `const title = payload.title.trim()` |
| `title` | `const` | [`packages/session/session-title/src/index.ts:595`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/session/session-title/src/index.ts#L595) | `const title = normalizeSessionTitle(candidate.title, this.config.maxTitleBytes)` |
| `title` | `const` | [`packages/session/session-title/src/index.ts:745`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/session/session-title/src/index.ts#L745) | `const title = fallbackSessionTitle(first.text, this.config.fallbackMaxWords, this.config.fallbackMaxBytes)` |
| `title` | `const` | [`packages/session/session-title/src/index.ts:761`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/session/session-title/src/index.ts#L761) | `const title = fallbackSessionTitle(` |
| `presentCall` | `function` | [`packages/workflow/tool-ralph/src/index.ts:394`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/workflow/tool-ralph/src/index.ts#L394) | `function presentCall(args: RalphCallArgs): ToolCallView {` |
| `presentResult` | `function` | [`packages/workflow/tool-ralph/src/index.ts:398`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/workflow/tool-ralph/src/index.ts#L398) | `function presentResult(args: RalphCallArgs, result: { content: ContentBlock[]; isError: boolean }): ToolResultView {` |

### Tests and executable evidence

- [`packages/workflow/tool-ralph/tests/tool-ralph.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/workflow/tool-ralph/tests/tool-ralph.spec.ts) — A test under the owning area exercises or imports `presentCall`. A test under the owning area exercises or imports `presentResult`.
- Source verification intent: packages/ui/tui/tests/tui.spec.ts pins the new header (Tool / ), the dropped diff title, the relocated generic title, and the · N file(s) footer. Package semantic snapshots cover the card families in a headless terminal. The deleted application journeys formerly supplied assembled tool executions; a future terminal deployment owns equivalent transcript coverage.

## How to read the implementation

1. Start with [`packages/client/web/src/app.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/web/src/app.tsx) because it has the strongest evidence link to the note.
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
- **Status gate** — Historical design. Use it to understand an older pressure or failure; locate the replacing decision before implementing anything.

### Smallest safe implementation slice

1. Express the semantic contract as Rust types and one owning boundary.
2. Add the smallest adapter or service needed by the existing loop; do not add a new model-facing tool unless the four-tool surface cannot express the job.
3. Record durable evidence before deriving summaries or UI state.
4. Add one happy-path test, one interruption test, and one replay/restart test where state survives a turn.
5. Keep the patch reversible until the source note's strongest failure case is proven in the Rust harness.

## Search handles

- Tags: `class/feature`, `concern/boundary`, `concern/evidence`, `concern/lifecycle`, `concern/ownership`, `concern/recovery`, `domain/build-release`, `domain/llm`, `domain/protocols`, `domain/session-state`, `domain/shell-terminal`, `domain/testing`, `domain/tools`, `domain/ui-interaction`, `lifecycle/archived`, `mechanism/policy`, `mechanism/projection`
- Aliases: `presentCall`, `presentResult`, `title`, `Tool / <name>`, `Read src/index.ts`, `{ring} Tool / <name>`, `. The ring marker is`, `/ <desc>`, `● Tool / bash / Run the coverage gate`, `ToolCardComponent`, `packages/ui/tui/src/components/transcript.ts`, `presentation.ts`, `Tool / read / Read src/index.ts`, `[exit N]`
- Regex: `(?i)(presentCall|presentResult|title|Tool[- ]/[- ]<name>|Read[- ]src/index\.ts|\{ring\}[- ]Tool[- ]/[- ]<name>|\.[- ]The[- ]ring[- ]marker[- ]is|/[- ]<desc>)`

```bash
rg -n --pcre2 "(?i)(presentCall|presentResult|title|Tool[- ]/[- ]<name>|Read[- ]src/index\\.ts|\\{ring\\}[- ]Tool[- ]/[- ]<name>|\\.[- ]The[- ]ring[- ]marker[- ]is|/[- ]<desc>)" source-deepseek-harness
rg -l --fixed-strings "class/feature" 03-agent-context
```

## Connected notes

- **`source-link`** — [0555. Consolidated TUI presentation and navigation](0555-consolidated-tui-presentation-and-navigation.md): The source note links to this decision directly.
- **`shares-code-with`** — [0366. Preset cards clamp their description instead of sizing the roster](0366-preset-cards-clamp-their-description-instead-of-sizing-the-roster.md): Shares source implementation: `packages/client/web/src/app.tsx`, `packages/host/apiproxy/src/api-proxy.ts`.
- **`shares-code-with`** — [0611. Keep the Code Mode result card complete](0611-keep-the-code-mode-result-card-complete.md): Shares source implementation: `packages/client/connection/src/client/fixture.ts`, `packages/workflow/tool-ralph/src/index.ts`.
- **`shares-code-with`** — [0407. CI failover runbook --- hosted pools → in-house pool](0407-ci-failover-runbook-hosted-pools-in-house-pool.md): Shares source implementation: `packages/client/connection/src/client/fixture.ts`, `packages/host/apiproxy/src/api-proxy.ts`.
- **`shares-code-with`** — [0194. Trajectory inspection ledger](0194-trajectory-inspection-ledger.md): Shares source implementation: `packages/client/connection/src/client/fixture.ts`, `packages/host/apiproxy/src/api-proxy.ts`.
- **`shares-code-with`** — [0617. Intent draft echoes in the same tick](0617-intent-draft-echoes-in-the-same-tick.md): Shares source implementation: `packages/client/web/src/app.tsx`, `packages/workflow/tool-ralph/src/index.ts`.
- **`shares-code-with`** — [0237. opening a produced file from the web UI](0237-opening-a-produced-file-from-the-web-ui.md): Shares source implementation: `packages/client/web/src/app.tsx`, `packages/host/apiproxy/src/api-proxy.ts`.
- **`shares-code-with`** — [0185. Workspace UI Complete Product Flow](0185-workspace-ui-complete-product-flow.md): Shares source implementation: `packages/client/web/src/app.tsx`, `packages/host/apiproxy/src/api-proxy.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0594-fixed-tool-name-header-for-tool-call-cards.md`.
