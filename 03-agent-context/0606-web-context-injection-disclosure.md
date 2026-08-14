---
id: "dsh-note-0606"
title: "Web context injection disclosure"
status: "archived"
class: "feature"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/archived/feature/2026-07-30-web-context-injection-disclosure.md"
implementation_evidence: "medium"
target_anchor: "Rust provider-adapter boundary"
tags:
  - "class/feature"
  - "concern/boundary"
  - "concern/evidence"
  - "concern/ownership"
  - "concern/recovery"
  - "domain/extensions"
  - "domain/llm"
  - "domain/session-state"
  - "domain/storage"
  - "domain/testing"
  - "lifecycle/archived"
  - "mechanism/policy"
  - "mechanism/registry"
aliases:
  - "source"
  - "ContextInjectionRow"
  - "DisclosureRow"
  - "JsonBlock"
  - "ToolRow"
  - "MessageItem"
  - "Web context injection disclosure"
  - "feature"
  - "boundary"
  - "evidence"
  - "ownership"
  - "recovery"
  - "extensions"
  - "llm"
search_regex: "(?i)(source|ContextInjectionRow|DisclosureRow|JsonBlock|ToolRow|MessageItem|Web[- ]context[- ]injection[- ]disclosure|feature)"
---

# 0606. Web context injection disclosure — implementation context

## Open this when

The Web conversation rendered every logged non-user message through the generic JsonBlock. That presentation used a textual triangle, compact label typography, a bordered JSON panel, and unrelated spacing, so context injection did not match the Tool calls disclosure shown in the product design. Restyling the generic primitive would also change unknown events and attachment fallbacks.

## Source decision

MessageItem routes context nodes to ContextInjectionRow. The row starts collapsed, names the presentation 上下文注入, uses the existing browse glyph, and exposes the whole 24px header as one pointer and keyboard disclosure target. Its expanded body begins 4px below the header at the shared 22px content indent and renders the design's 141px scrollport with 8px radius, code-block background, 11/16 code text, and no border. ContextInjectionRow serializes both logged content and source into one inline JSON value, preserving provenance alongside model-visible material.

## Decision status

Historical design. Use it to understand an older pressure or failure; locate the replacing decision before implementing anything.

- Raw note: [archived/feature/2026-07-30-web-context-injection-disclosure.md](../02-notes/archived/feature/2026-07-30-web-context-injection-disclosure.md)
- Pinned source: [.agents/notes/archived/feature/2026-07-30-web-context-injection-disclosure.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/archived/feature/2026-07-30-web-context-injection-disclosure.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`vendor/hmr/src/error.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/hmr/src/error.ts) | runtime implementation | Defines `source`, a construct named by the note. | `symbol-definition` |
| [`packages/llm/llm/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/src/index.ts) | package entry point | Defines `source`, a construct named by the note. | `symbol-definition` |
| [`packages/goal/goal/src/fold.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/goal/goal/src/fold.ts) | runtime implementation | Defines `source`, a construct named by the note. | `symbol-definition` |
| [`vendor/loader/src/config/tree.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/loader/src/config/tree.ts) | runtime implementation | Defines `source`, a construct named by the note. | `symbol-definition` |
| [`packages/api/gateway/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/api/gateway/src/index.ts) | package entry point | Defines `source`, a construct named by the note. | `symbol-definition` |
| [`packages/client/ui-primitives/src/DisclosureRow.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-primitives/src/DisclosureRow.tsx) | runtime implementation | Defines `DisclosureRow`, a construct named by the note. | `symbol-definition` |
| [`packages/client/ui-primitives/src/markdown/JsonBlock.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-primitives/src/markdown/JsonBlock.tsx) | runtime implementation | Defines `JsonBlock`, a construct named by the note. | `symbol-definition` |
| [`packages/client/ui-tool/src/client/tool/components/ToolRow.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-tool/src/client/tool/components/ToolRow.tsx) | runtime implementation | Defines `ToolRow`, a construct named by the note. | `symbol-definition` |
| [`packages/client/ui-conversation/src/client/chat/ContextInjectionRow.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/src/client/chat/ContextInjectionRow.tsx) | runtime implementation | Defines `ContextInjectionRow`, a construct named by the note. | `symbol-definition` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `source` | `const` | [`packages/api/gateway/src/index.ts:562`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/api/gateway/src/index.ts#L562) | `const source = Function.prototype.toString.call(implementation)` |
| `ContextInjectionRow` | `function` | [`packages/client/ui-conversation/src/client/chat/ContextInjectionRow.tsx:31`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/src/client/chat/ContextInjectionRow.tsx#L31) | `export function ContextInjectionRow({ content, source, provenance, form, t }: ContextInjectionRowProps) {` |
| `DisclosureRow` | `function` | [`packages/client/ui-primitives/src/DisclosureRow.tsx:33`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-primitives/src/DisclosureRow.tsx#L33) | `export function DisclosureRow({` |
| `JsonBlock` | `function` | [`packages/client/ui-primitives/src/markdown/JsonBlock.tsx:13`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-primitives/src/markdown/JsonBlock.tsx#L13) | `export function JsonBlock({ label, payload, defaultOpen = false, truncatedLabel = defaultTruncatedLabel }: {` |
| `ToolRow` | `function` | [`packages/client/ui-tool/src/client/tool/components/ToolRow.tsx:128`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-tool/src/client/tool/components/ToolRow.tsx#L128) | `export function ToolRow({` |
| `source` | `const` | [`packages/goal/goal/src/fold.ts:322`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/goal/goal/src/fold.ts#L322) | `const source = goalSource(event.data.source)` |
| `source` | `const` | [`packages/llm/llm/src/index.ts:825`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/src/index.ts#L825) | `const source = message.source` |
| `source` | `const` | [`vendor/hmr/src/error.ts:24`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/hmr/src/error.ts#L24) | `const source = readFileSync(file, 'utf8')` |
| `source` | `const` | [`vendor/loader/src/config/tree.ts:116`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/loader/src/config/tree.ts#L116) | `const source = entry.parent` |

### Tests and executable evidence

- [`packages/client/ui-tool/tests/web-card.client.spec.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-tool/tests/web-card.client.spec.tsx) — A test under the owning area exercises or imports `ToolRow`.
- [`packages/client/ui-tool/tests/tool-row.client.spec.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-tool/tests/tool-row.client.spec.tsx) — A test under the owning area exercises or imports `ToolRow`.
- [`packages/client/ui-tool/tests/diff-card.client.spec.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-tool/tests/diff-card.client.spec.tsx) — A test under the owning area exercises or imports `ToolRow`.
- [`packages/client/ui-tool/tests/read-card.client.spec.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-tool/tests/read-card.client.spec.tsx) — A test under the owning area exercises or imports `ToolRow`.
- [`packages/client/ui-tool/tests/search-card.client.spec.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-tool/tests/search-card.client.spec.tsx) — A test under the owning area exercises or imports `ToolRow`.
- [`packages/client/ui-tool/tests/terminal-card.client.spec.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-tool/tests/terminal-card.client.spec.tsx) — A test under the owning area exercises or imports `ToolRow`.
- [`packages/client/ui-primitives/tests/markdown.client.spec.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-primitives/tests/markdown.client.spec.tsx) — A test under the owning area exercises or imports `JsonBlock`.
- [`packages/client/ui-tool/tests/tool-row-styles.client.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-tool/tests/tool-row-styles.client.spec.ts) — A test under the owning area exercises or imports `ToolRow`.
- Source verification intent: Conversation component tests pin the collapsed default, browse glyph, whole-row pointer and keyboard toggles, inline JSON shape, truncation, and unchanged generic unknown-event rendering. The keyless assembled-Web history scenario injects context through the real Agent API, records the collapsed row in its ARIA golden, and measures the design's icon, header, indent, gap, scrollport, padding, radius, typography, color, and overflow in Chromium.

## How to read the implementation

1. Start with [`vendor/hmr/src/error.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/hmr/src/error.ts) because it has the strongest evidence link to the note.
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
- **`rust/quickjs-boundary`** — QuickJS holds orchestration variables; Rust/Tokio owns async execution and exposes only explicitly granted capabilities.
- **`rust/trait-seam`** — Use a small trait at the capability boundary; keep policy and model-facing schemas outside the backend implementation.
- **Status gate** — Historical design. Use it to understand an older pressure or failure; locate the replacing decision before implementing anything.

### Smallest safe implementation slice

1. Express the semantic contract as Rust types and one owning boundary.
2. Add the smallest adapter or service needed by the existing loop; do not add a new model-facing tool unless the four-tool surface cannot express the job.
3. Record durable evidence before deriving summaries or UI state.
4. Add one happy-path test, one interruption test, and one replay/restart test where state survives a turn.
5. Keep the patch reversible until the source note's strongest failure case is proven in the Rust harness.

## Search handles

- Tags: `class/feature`, `concern/boundary`, `concern/evidence`, `concern/ownership`, `concern/recovery`, `domain/extensions`, `domain/llm`, `domain/session-state`, `domain/storage`, `domain/testing`, `lifecycle/archived`, `mechanism/policy`, `mechanism/registry`
- Aliases: `source`, `ContextInjectionRow`, `DisclosureRow`, `JsonBlock`, `ToolRow`, `MessageItem`, `Web context injection disclosure`, `feature`, `boundary`, `evidence`, `ownership`, `recovery`, `extensions`, `llm`
- Regex: `(?i)(source|ContextInjectionRow|DisclosureRow|JsonBlock|ToolRow|MessageItem|Web[- ]context[- ]injection[- ]disclosure|feature)`

```bash
rg -n --pcre2 "(?i)(source|ContextInjectionRow|DisclosureRow|JsonBlock|ToolRow|MessageItem|Web[- ]context[- ]injection[- ]disclosure|feature)" source-deepseek-harness
rg -l --fixed-strings "class/feature" 03-agent-context
```

## Connected notes

- **`shares-code-with`** — [0200. Continuable subagents](0200-continuable-subagents.md): Shares source implementation: `packages/goal/goal/src/fold.ts`, `packages/llm/llm/src/index.ts`.
- **`shares-code-with`** — [0058. Separate context injection from turn execution](0058-separate-context-injection-from-turn-execution.md): Shares source implementation: `packages/goal/goal/src/fold.ts`, `packages/llm/llm/src/index.ts`.
- **`shares-code-with`** — [0659. Remove the `agent/steering` mirror emit](0659-remove-the-agent-steering-mirror-emit.md): Shares source implementation: `packages/goal/goal/src/fold.ts`, `packages/llm/llm/src/index.ts`.
- **`shares-code-with`** — [0053. Unify agent delivery on send(target × wakeup) and coalesce injected context into user/message](0053-unify-agent-delivery-on-send-target-wakeup-and-coalesce-injected-context.md): Shares source implementation: `packages/llm/llm/src/index.ts`, `vendor/hmr/src/error.ts`.
- **`shares-code-with`** — [0088. Claim inbox input before one pre-step decision](0088-claim-inbox-input-before-one-pre-step-decision.md): Shares source implementation: `packages/goal/goal/src/fold.ts`, `packages/llm/llm/src/index.ts`.
- **`shares-code-with`** — [0078. Terminal LLM stream failures](0078-terminal-llm-stream-failures.md): Shares source implementation: `packages/goal/goal/src/fold.ts`, `packages/llm/llm/src/index.ts`.
- **`shares-code-with`** — [0560. Subagent lifecycle enrichment --- lastAssistantMessage (observe-only)](0560-subagent-lifecycle-enrichment-lastassistantmessage-observe-only.md): Shares source implementation: `packages/api/gateway/src/index.ts`, `packages/llm/llm/src/index.ts`.
- **`shares-code-with`** — [0244. Web thinking tail scroll --- collapsed reasoning follows live output](0244-web-thinking-tail-scroll-collapsed-reasoning-follows-live-output.md): Shares source implementation: `packages/client/ui-tool/src/client/tool/components/ToolRow.tsx`, `packages/client/ui-tool/tests/web-card.client.spec.tsx`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0606-web-context-injection-disclosure.md`.
