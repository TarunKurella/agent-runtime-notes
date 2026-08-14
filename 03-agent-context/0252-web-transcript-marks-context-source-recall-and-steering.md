---
id: "dsh-note-0252"
title: "Web transcript marks context source, recall, and steering"
status: "implemented"
class: "feature"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/feature/2026-08-04-web-context-source-and-steer-marks.md"
implementation_evidence: "medium"
target_anchor: "Rust provider-adapter boundary"
tags:
  - "class/feature"
  - "concern/boundary"
  - "concern/compatibility"
  - "concern/concurrency"
  - "concern/discovery-routing"
  - "concern/evidence"
  - "concern/human-control"
  - "concern/lifecycle"
  - "concern/ownership"
  - "concern/schema-types"
  - "concern/simplification"
  - "domain/agent-loop"
  - "domain/build-release"
  - "domain/context"
  - "domain/extensions"
  - "domain/llm"
  - "domain/multi-agent"
  - "domain/session-state"
  - "domain/shell-terminal"
  - "domain/storage"
  - "domain/testing"
  - "domain/ui-interaction"
  - "lifecycle/implemented"
  - "mechanism/event-sourcing"
  - "mechanism/policy"
  - "mechanism/projection"
  - "mechanism/registry"
aliases:
  - "contextProvenance"
  - "UserMessageNode"
  - "SteeringMessageNode"
  - "ContextMessageNode"
  - "ContextInjectionRow"
  - "ToolRow"
  - "inject"
  - "label"
  - "user/message"
  - "AGENTS.md"
  - "user/message.source"
  - "agent/inbox/spliced"
  - "next-turn"
  - "next-step"
search_regex: "(?i)(contextProvenance|UserMessageNode|SteeringMessageNode|ContextMessageNode|ContextInjectionRow|ToolRow|inject|label)"
---

# 0252. Web transcript marks context source, recall, and steering — implementation context

## Open this when

Everything a producer adds to the model-facing conversation reached the Web transcript as one of two anonymous shapes. Every logged non-user user/message --- the skill catalog, the runtime snapshot, reconciled AGENTS.md instructions, a guard notice, a subagent report, a cross-session snapshot --- collapsed into one identical 上下文注入 row, so a reader could not tell what had been added without expanding each row and reading raw JSON. Mid-turn steering was worse: it rendered in exactly the bubble a turn-opening prompt uses, leaving the transcript unable to say which message interrupted a running turn.

## Source decision

The transcript names all three roles a non-prompt message can play --- injected context, recalled session, and steering. The Chat Message Definition attaches a provenance view containing the producer role and label to every ContextMessageNode; contextProvenance() computes it from the durable source alone. It returns a role (inject, or recall for a cross-session snapshot) and a label naming the producer.

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/feature/2026-08-04-web-context-source-and-steer-marks.md](../02-notes/implemented/feature/2026-08-04-web-context-source-and-steer-marks.md)
- Pinned source: [.agents/notes/implemented/feature/2026-08-04-web-context-source-and-steer-marks.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/feature/2026-08-04-web-context-source-and-steer-marks.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`packages/skill/tool-skill/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/skill/tool-skill/src/index.ts) | package entry point | Core file in the package named by the note: `packages/skill/tool-skill`. Defines `inject`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/core/system-prompt/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/system-prompt/src/index.ts) | package entry point | Core file in the package named by the note: `packages/core/system-prompt`. | `named-package-member` |
| [`packages/skill/tool-skill/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/skill/tool-skill/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/skill/tool-skill`. Defines `inject`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/core/system-prompt/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/system-prompt/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/core/system-prompt`. Defines `inject`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/context/session-reference/src/uri.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/context/session-reference/src/uri.ts) | runtime implementation | Core file in the package named by the note: `packages/context/session-reference`. Defines `label`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/context/session-reference/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/context/session-reference/src/index.ts) | package entry point | Core file in the package named by the note: `packages/context/session-reference`. | `named-package-member` |
| [`packages/context/session-reference/src/types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/context/session-reference/src/types.ts) | public types and contract | Core file in the package named by the note: `packages/context/session-reference`. | `named-package-member` |
| [`packages/context/agent-instructions/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/context/agent-instructions/src/index.ts) | package entry point | Core file in the package named by the note: `packages/context/agent-instructions`. | `named-package-member` |
| [`packages/context/session-reference/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/context/session-reference/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/context/session-reference`. Defines `inject`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/context/agent-instructions/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/context/agent-instructions/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/context/agent-instructions`. Defines `inject`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/client/runtime/README.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/runtime/README.md) | package contract and examples | Entry point or contract under the directory named by the note: `packages/client/runtime`. | `named-directory-member` |
| [`packages/client/runtime/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/runtime/src/index.ts) | package entry point | Entry point or contract under the directory named by the note: `packages/client/runtime`. | `named-directory-member` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `contextProvenance` | `function` | [`packages/client/runtime/src/client/sessions/context-provenance.ts:71`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/runtime/src/client/sessions/context-provenance.ts#L71) | `export function contextProvenance(source: unknown): ContextProvenanceView {` |
| `UserMessageNode` | `interface` | [`packages/client/runtime/src/client/sessions/conversation.ts:76`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/runtime/src/client/sessions/conversation.ts#L76) | `export interface UserMessageNode {` |
| `SteeringMessageNode` | `interface` | [`packages/client/runtime/src/client/sessions/conversation.ts:121`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/runtime/src/client/sessions/conversation.ts#L121) | `export interface SteeringMessageNode {` |
| `ContextMessageNode` | `interface` | [`packages/client/runtime/src/client/sessions/conversation.ts:133`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/runtime/src/client/sessions/conversation.ts#L133) | `export interface ContextMessageNode {` |
| `ContextInjectionRow` | `function` | [`packages/client/ui-conversation/src/client/chat/ContextInjectionRow.tsx:31`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/src/client/chat/ContextInjectionRow.tsx#L31) | `export function ContextInjectionRow({ content, source, provenance, form, t }: ContextInjectionRowProps) {` |
| `ToolRow` | `function` | [`packages/client/ui-tool/src/client/tool/components/ToolRow.tsx:128`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-tool/src/client/tool/components/ToolRow.tsx#L128) | `export function ToolRow({` |
| `inject` | `const` | [`packages/context/agent-instructions/src/invariant.ts:15`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/context/agent-instructions/src/invariant.ts#L15) | `export const inject = ['invariants']` |
| `inject` | `const` | [`packages/context/session-reference/src/invariant.ts:15`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/context/session-reference/src/invariant.ts#L15) | `export const inject = ['invariants']` |
| `label` | `const` | [`packages/context/session-reference/src/uri.ts:48`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/context/session-reference/src/uri.ts#L48) | `const label = escapeLabel(reference.label ?? reference.sessionId)` |
| `label` | `const` | [`packages/context/session-reference/src/uri.ts:81`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/context/session-reference/src/uri.ts#L81) | `const label = rawLabel === undefined ? sessionId : unescapeLabel(rawLabel)` |
| `inject` | `const` | [`packages/core/system-prompt/src/invariant.ts:13`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/system-prompt/src/invariant.ts#L13) | `export const inject = ['invariants']` |
| `inject` | `const` | [`packages/skill/tool-skill/src/index.ts:25`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/skill/tool-skill/src/index.ts#L25) | `export const inject = ['agents', 'tools', 'skills']` |
| `inject` | `const` | [`packages/skill/tool-skill/src/invariant.ts:15`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/skill/tool-skill/src/invariant.ts#L15) | `export const inject = ['invariants']` |

### Tests and executable evidence

- [`packages/skill/tool-skill/tests/tool-skill.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/skill/tool-skill/tests/tool-skill.spec.ts) — A test under the owning area exercises or imports `dsh-tool-skill`.
- [`packages/client/ui-tool/tests/web-card.client.spec.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-tool/tests/web-card.client.spec.tsx) — A test under the owning area exercises or imports `ToolRow`.
- [`packages/client/ui-tool/tests/tool-row.client.spec.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-tool/tests/tool-row.client.spec.tsx) — A test under the owning area exercises or imports `ToolRow`.
- [`packages/client/ui-tool/tests/diff-card.client.spec.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-tool/tests/diff-card.client.spec.tsx) — A test under the owning area exercises or imports `ToolRow`.
- [`packages/client/ui-tool/tests/read-card.client.spec.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-tool/tests/read-card.client.spec.tsx) — A test under the owning area exercises or imports `ToolRow`.
- [`packages/client/ui-tool/tests/search-card.client.spec.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-tool/tests/search-card.client.spec.tsx) — A test under the owning area exercises or imports `ToolRow`.
- [`packages/client/ui-tool/tests/terminal-card.client.spec.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-tool/tests/terminal-card.client.spec.tsx) — A test under the owning area exercises or imports `ToolRow`.
- [`packages/client/ui-tool/tests/tool-row-styles.client.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-tool/tests/tool-row-styles.client.spec.ts) — A test under the owning area exercises or imports `ToolRow`.
- Source verification intent: packages/client/runtime unit coverage pins each source kind, the label fallbacks when a name field is missing, empty, or wrongly typed, the unnamed degradation for a source with no readable kind, and steering reconstruction on reset and live append paths. packages/client/ui-conversation jsdom coverage pins the role title, the producer label beside it, the label's survival while expanded, and the roleless header. The keyless assembled-Web goldens carry the named header, so the assembled transcript --- not only component tests --- proves the marks.

## How to read the implementation

1. Start with [`packages/skill/tool-skill/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/skill/tool-skill/src/index.ts) because it has the strongest evidence link to the note.
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

- Tags: `class/feature`, `concern/boundary`, `concern/compatibility`, `concern/concurrency`, `concern/discovery-routing`, `concern/evidence`, `concern/human-control`, `concern/lifecycle`, `concern/ownership`, `concern/schema-types`, `concern/simplification`, `domain/agent-loop`, `domain/build-release`, `domain/context`, `domain/extensions`, `domain/llm`, `domain/multi-agent`, `domain/session-state`, `domain/shell-terminal`, `domain/storage`, `domain/testing`, `domain/ui-interaction`, `lifecycle/implemented`, `mechanism/event-sourcing`, `mechanism/policy`, `mechanism/projection`, `mechanism/registry`
- Aliases: `contextProvenance`, `UserMessageNode`, `SteeringMessageNode`, `ContextMessageNode`, `ContextInjectionRow`, `ToolRow`, `inject`, `label`, `user/message`, `AGENTS.md`, `user/message.source`, `agent/inbox/spliced`, `next-turn`, `next-step`
- Regex: `(?i)(contextProvenance|UserMessageNode|SteeringMessageNode|ContextMessageNode|ContextInjectionRow|ToolRow|inject|label)`

```bash
rg -n --pcre2 "(?i)(contextProvenance|UserMessageNode|SteeringMessageNode|ContextMessageNode|ContextInjectionRow|ToolRow|inject|label)" source-deepseek-harness
rg -l --fixed-strings "class/feature" 03-agent-context
```

## Connected notes

- **`source-link`** — [0606. Web context injection disclosure](0606-web-context-injection-disclosure.md): The source note links to this decision directly.
- **`source-link`** — [0675. Web UI drops steer entry and interjection chrome](0675-web-ui-drops-steer-entry-and-interjection-chrome.md): The source note links to this decision directly.
- **`source-link`** — [0256. Producer-declared context forms](0256-producer-declared-context-forms.md): The source note links to this decision directly.
- **`source-link`** — [0486. Remove the steering interjection caption](0486-remove-the-steering-interjection-caption.md): The source note links to this decision directly.
- **`shares-code-with`** — [0277. User-explicit skill invocation at the pre-step gesture boundary](0277-user-explicit-skill-invocation-at-the-pre-step-gesture-boundary.md): Shares source implementation: `packages/context/agent-instructions/src/index.ts`, `packages/context/agent-instructions/src/invariant.ts`.
- **`shares-code-with`** — [0136. Workspace context instruction files](0136-workspace-context-instruction-files.md): Shares source implementation: `packages/context/agent-instructions/src/index.ts`, `packages/context/agent-instructions/src/invariant.ts`.
- **`shares-code-with`** — [0036. Shared scoped-layer storage](0036-shared-scoped-layer-storage.md): Shares source implementation: `packages/core/system-prompt/src/index.ts`, `packages/core/system-prompt/src/invariant.ts`.
- **`shares-code-with`** — [0212. Current sandbox policy context](0212-current-sandbox-policy-context.md): Shares source implementation: `packages/core/system-prompt/src/index.ts`, `packages/core/system-prompt/src/invariant.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0252-web-transcript-marks-context-source-recall-and-steering.md`.
