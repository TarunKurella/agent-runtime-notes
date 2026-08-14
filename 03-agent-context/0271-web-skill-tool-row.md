---
id: "dsh-note-0271"
title: "Web skill tool row"
status: "implemented"
class: "feature"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/feature/2026-08-06-web-skill-tool-row.md"
implementation_evidence: "medium"
target_anchor: "Rust provider-adapter boundary"
tags:
  - "class/feature"
  - "concern/boundary"
  - "concern/lifecycle"
  - "concern/ownership"
  - "concern/recovery"
  - "domain/llm"
  - "domain/protocols"
  - "domain/session-state"
  - "domain/shell-terminal"
  - "domain/storage"
  - "domain/tools"
  - "domain/ui-interaction"
  - "lifecycle/implemented"
  - "mechanism/event-sourcing"
aliases:
  - "ToolCallViewProps"
  - "ToolRow"
  - "skill"
  - "ui-skill"
  - "tool.call.toolview"
  - "skill-load"
  - "ui-conversation"
  - "<skill_instructions>"
  - "Web skill tool row"
  - "feature"
  - "boundary"
  - "lifecycle"
  - "ownership"
  - "recovery"
search_regex: "(?i)(ToolCallViewProps|ToolRow|skill|ui\\-skill|tool\\.call\\.toolview|skill\\-load|ui\\-conversation|<skill_instructions>)"
---

# 0271. Web skill tool row — implementation context

## Open this when

The Web transcript renders skill calls through the generic fallback row, so a loaded instruction set looks like an unknown tool call even though Skill is a first-class product concept. The generic row also exposes the JSON argument envelope beside the result, adding noise around the one identity users need: the loaded skill name.

## Source decision

ui-skill registers a component under ui-tool's tool.call.toolview keyed slot with key skill. The component consumes the public ToolCallViewProps owner contract and owns its row chrome without importing ui-tool presentation internals. The collapsed row uses a 14-pixel document-and-sparkle glyph and the Bash row's neutral hierarchy: tertiary glyph, secondary Skill title, caption separator, and tertiary skill name. Running, failed, and interrupted calls retain the transcript's shimmer, error dot and first-line summary, and warning dot semantics.

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/feature/2026-08-06-web-skill-tool-row.md](../02-notes/implemented/feature/2026-08-06-web-skill-tool-row.md)
- Pinned source: [.agents/notes/implemented/feature/2026-08-06-web-skill-tool-row.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/feature/2026-08-06-web-skill-tool-row.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`packages/skill/skill/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/skill/skill/src/index.ts) | package entry point | Core file in the package named by the note: `packages/skill/skill`. Defines `skill`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/client/ui-skill/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-skill/src/index.ts) | package entry point | Core file in the package named by the note: `packages/client/ui-skill`. | `named-package-member` |
| [`packages/skill/skill/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/skill/skill/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/skill/skill`. | `named-package-member` |
| [`packages/client/ui-skill/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-skill/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/client/ui-skill`. | `named-package-member` |
| [`packages/client/ui-conversation/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/src/index.ts) | package entry point | Core file in the package named by the note: `packages/client/ui-conversation`. | `named-package-member` |
| [`packages/client/ui-conversation/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/client/ui-conversation`. | `named-package-member` |
| [`packages/client/ui-skill/src/client/SkillRow.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-skill/src/client/SkillRow.tsx) | runtime implementation | Core file in the package named by the note: `packages/client/ui-skill`. | `named-package-member` |
| [`packages/client/ui-conversation/src/client/input/decorations.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/src/client/input/decorations.ts) | runtime implementation | Core file in the package named by the note: `packages/client/ui-conversation`. | `named-package-member` |
| [`packages/client/ui-conversation/src/client/chat/ContextBody.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/src/client/chat/ContextBody.tsx) | runtime implementation | Core file in the package named by the note: `packages/client/ui-conversation`. | `named-package-member` |
| [`packages/skill/skill`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/packages/skill/skill) | package or module directory | The note names this package or capability. | `named-package` |
| [`packages/client/ui-skill`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-skill) | package or module directory | The note names this package or capability. | `named-package` |
| [`packages/client/ui-conversation`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation) | package or module directory | The note names this package or capability. | `named-package` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `ToolCallViewProps` | `type` | [`packages/client/ui-tool/src/client/contract/slots.ts:44`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-tool/src/client/contract/slots.ts#L44) | `export type ToolCallViewProps = PropsRuntime<'tool.call.toolview'>` |
| `ToolRow` | `function` | [`packages/client/ui-tool/src/client/tool/components/ToolRow.tsx:128`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-tool/src/client/tool/components/ToolRow.tsx#L128) | `export function ToolRow({` |
| `skill` | `const` | [`packages/skill/skill/src/index.ts:574`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/skill/skill/src/index.ts#L574) | `const skill = entry.candidate` |

### Tests and executable evidence

- [`packages/skill/skill/tests/skill.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/skill/skill/tests/skill.spec.ts) — A test under the owning area exercises or imports `Skill`.
- [`packages/client/ui-tool/tests/web-card.client.spec.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-tool/tests/web-card.client.spec.tsx) — A test under the owning area exercises or imports `ToolRow`.
- [`packages/client/ui-tool/tests/tool-row.client.spec.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-tool/tests/tool-row.client.spec.tsx) — A test under the owning area exercises or imports `ToolRow`.
- [`packages/client/ui-tool/tests/diff-card.client.spec.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-tool/tests/diff-card.client.spec.tsx) — A test under the owning area exercises or imports `ToolRow`.
- [`packages/client/ui-tool/tests/read-card.client.spec.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-tool/tests/read-card.client.spec.tsx) — A test under the owning area exercises or imports `ToolRow`.
- [`packages/client/ui-skill/tests/skill-row.client.spec.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-skill/tests/skill-row.client.spec.tsx) — A test under the owning area exercises or imports `Inspect`.
- [`packages/client/ui-tool/tests/search-card.client.spec.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-tool/tests/search-card.client.spec.tsx) — A test under the owning area exercises or imports `ToolRow`.
- [`packages/client/ui-tool/tests/toolview-slot.client.spec.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-tool/tests/toolview-slot.client.spec.tsx) — A test under the owning area exercises or imports `ToolCallViewProps`.

## How to read the implementation

1. Start with [`packages/skill/skill/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/skill/skill/src/index.ts) because it has the strongest evidence link to the note.
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

- Tags: `class/feature`, `concern/boundary`, `concern/lifecycle`, `concern/ownership`, `concern/recovery`, `domain/llm`, `domain/protocols`, `domain/session-state`, `domain/shell-terminal`, `domain/storage`, `domain/tools`, `domain/ui-interaction`, `lifecycle/implemented`, `mechanism/event-sourcing`
- Aliases: `ToolCallViewProps`, `ToolRow`, `skill`, `ui-skill`, `tool.call.toolview`, `skill-load`, `ui-conversation`, `<skill_instructions>`, `Web skill tool row`, `feature`, `boundary`, `lifecycle`, `ownership`, `recovery`
- Regex: `(?i)(ToolCallViewProps|ToolRow|skill|ui\-skill|tool\.call\.toolview|skill\-load|ui\-conversation|<skill_instructions>)`

```bash
rg -n --pcre2 "(?i)(ToolCallViewProps|ToolRow|skill|ui\\-skill|tool\\.call\\.toolview|skill\\-load|ui\\-conversation|<skill_instructions>)" source-deepseek-harness
rg -l --fixed-strings "class/feature" 03-agent-context
```

## Connected notes

- **`shares-code-with`** — [0192. Skill catalog hot refresh](0192-skill-catalog-hot-refresh.md): Shares source implementation: `packages/skill/skill`, `packages/skill/skill/src/index.ts`.
- **`shares-code-with`** — [0277. User-explicit skill invocation at the pre-step gesture boundary](0277-user-explicit-skill-invocation-at-the-pre-step-gesture-boundary.md): Shares source implementation: `packages/skill/skill`, `packages/skill/skill/src/index.ts`.
- **`shares-code-with`** — [0580. TUI skill slash command](0580-tui-skill-slash-command.md): Shares source implementation: `packages/skill/skill`, `packages/skill/skill/src/index.ts`.
- **`shares-code-with`** — [0177. Safe assistant Markdown in the Web conversation](0177-safe-assistant-markdown-in-the-web-conversation.md): Shares source implementation: `packages/client/ui-conversation/src/index.ts`, `packages/client/ui-conversation/src/invariant.ts`.
- **`shares-code-with`** — [0049. Canonical tool output contract](0049-canonical-tool-output-contract.md): Shares source implementation: `packages/skill/skill/src/index.ts`, `packages/skill/skill/src/invariant.ts`.
- **`shares-code-with`** — [0520. Task Surface for structured session interaction](0520-task-surface-for-structured-session-interaction.md): Shares source implementation: `packages/client/ui-conversation/src/index.ts`, `packages/client/ui-conversation/src/invariant.ts`.
- **`shares-code-with`** — [0208. Ask-question Web presentation](0208-ask-question-web-presentation.md): Shares source implementation: `packages/client/ui-conversation/src/index.ts`, `packages/client/ui-conversation/src/invariant.ts`.
- **`shares-code-with`** — [0143. Skill system --- progressive disclosure instructions for agents](0143-skill-system-progressive-disclosure-instructions-for-agents.md): Shares source implementation: `packages/skill/skill/src/index.ts`, `packages/skill/skill/src/invariant.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0271-web-skill-tool-row.md`.
