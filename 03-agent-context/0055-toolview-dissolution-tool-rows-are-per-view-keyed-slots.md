---
id: "dsh-note-0055"
title: "Toolview dissolution --- tool rows are per-view keyed slots"
status: "implemented"
class: "architecture"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/architecture/2026-07-23-toolview-dissolution.md"
implementation_evidence: "medium"
target_anchor: "Rust provider-adapter boundary"
tags:
  - "class/architecture"
  - "concern/boundary"
  - "concern/compatibility"
  - "concern/discovery-routing"
  - "concern/evidence"
  - "concern/ownership"
  - "concern/performance"
  - "concern/recovery"
  - "concern/schema-types"
  - "domain/build-release"
  - "domain/llm"
  - "domain/session-state"
  - "lifecycle/implemented"
  - "mechanism/policy"
  - "mechanism/projection"
  - "mechanism/registry"
aliases:
  - "inject"
  - "SlotMap"
  - "useSessions"
  - "entryKey"
  - "ctx.toolviews"
  - "conversation.chat.toolview"
  - "ui-tool"
  - "tool.call.toolview"
  - "renderToolView"
  - "registerToolView"
  - "ctx.slots.inject"
  - "Toolview dissolution --- tool rows are per-view keyed slots"
  - "architecture"
  - "boundary"
search_regex: "(?i)(inject|SlotMap|useSessions|entryKey|ctx\\.toolviews|conversation\\.chat\\.toolview|ui\\-tool|tool\\.call\\.toolview)"
---

# 0055. Toolview dissolution --- tool rows are per-view keyed slots — implementation context

## Open this when

After the view ring dissolved into the slot system, the client kept exactly one parallel registration model: the tool ring --- a named registry (ctx.toolviews) with its own register grammar, its own resolve semantics (scoped-beats-global predicate dispatch), its own subscribe/version pair, its own inject cache, and its own render outlet with a private error boundary. Every one of those was a second implementation of something the slot machinery already owned, and every future capability (a store seat for row drafts, i18n injection, cross-bundle identity) would have had to be built twice or drift.

## Source decision

The tool ring is gone as independent infrastructure: a tool row is a keyed child slot each view declares for itself, and the client has exactly one registration model. The justification above was hollow --- a keyed slot's key space is already runtime-open (SlotMap declares slots, never keys; the ask-user composer's key: 'question' was the precedent), so the open tool-name set fits entryKey dispatch natively. This decision originally placed 'conversation.chat.toolview' under the chat entry and made the chat render site dispatch each row.

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/architecture/2026-07-23-toolview-dissolution.md](../02-notes/implemented/architecture/2026-07-23-toolview-dissolution.md)
- Pinned source: [.agents/notes/implemented/architecture/2026-07-23-toolview-dissolution.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/architecture/2026-07-23-toolview-dissolution.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`packages/client/ui-tool/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-tool/src/index.ts) | package entry point | Core file in the package named by the note: `packages/client/ui-tool`. | `named-package-member` |
| [`packages/client/ui-tool/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-tool/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/client/ui-tool`. Defines `inject`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/client/ui-tool/src/client/apply.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-tool/src/client/apply.ts) | runtime implementation | Core file in the package named by the note: `packages/client/ui-tool`. Defines `inject`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/client/ui-tool/src/client/contract/slots.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-tool/src/client/contract/slots.ts) | runtime implementation | Core file in the package named by the note: `packages/client/ui-tool`. Defines `SlotMap`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/client/ui-tool`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-tool) | package or module directory | The note names this package or capability. | `named-package` |
| [`scripts/slot-walk.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/slot-walk.ts) | repository automation | Defines `entryKey`, a construct named by the note. | `symbol-definition` |
| [`packages/client/web/src/app.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/web/src/app.tsx) | runtime implementation | Defines `useSessions`, a construct named by the note. | `symbol-definition` |
| [`packages/client/ui-tool/README.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-tool/README.md) | package contract and examples | Core file in the package named by the note: `packages/client/ui-tool`. | `named-package-member` |
| [`packages/client/ui-tool/package.json`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-tool/package.json) | composition and configuration | Core file in the package named by the note: `packages/client/ui-tool`. | `named-package-member` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `inject` | `const` | [`packages/client/ui-tool/src/client/apply.ts:16`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-tool/src/client/apply.ts#L16) | `export const inject = ['slots']` |
| `SlotMap` | `interface` | [`packages/client/ui-tool/src/client/contract/slots.ts:8`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-tool/src/client/contract/slots.ts#L8) | `interface SlotMap {` |
| `inject` | `const` | [`packages/client/ui-tool/src/invariant.ts:15`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-tool/src/invariant.ts#L15) | `export const inject = ['invariants']` |
| `useSessions` | `const` | [`packages/client/web/src/app.tsx:30`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/web/src/app.tsx#L30) | `const useSessions = bindSnapshotSelector(sessions.list)` |
| `entryKey` | `const` | [`scripts/slot-walk.ts:207`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/slot-walk.ts#L207) | `const entryKey = stringProperty(options, 'key')` |

### Tests and executable evidence

- [`packages/client/ui-tool/tests/todo-row.client.spec.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-tool/tests/todo-row.client.spec.tsx) — A test under the owning area exercises or imports `toolviews`. A test under the owning area exercises or imports `toolview`.
- [`packages/client/ui-tool/tests/web-card.client.spec.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-tool/tests/web-card.client.spec.tsx) — A test under the owning area exercises or imports `toolviews`. A test under the owning area exercises or imports `toolview`.
- [`packages/client/ui-tool/tests/tool-row.client.spec.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-tool/tests/tool-row.client.spec.tsx) — A test under the owning area exercises or imports `toolviews`. A test under the owning area exercises or imports `toolview`.
- [`packages/client/ui-tool/tests/diff-card.client.spec.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-tool/tests/diff-card.client.spec.tsx) — A test under the owning area exercises or imports `toolviews`. A test under the owning area exercises or imports `useSessions`.
- [`packages/client/ui-tool/tests/read-card.client.spec.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-tool/tests/read-card.client.spec.tsx) — A test under the owning area exercises or imports `toolviews`. A test under the owning area exercises or imports `toolview`.
- [`packages/client/ui-tool/tests/search-card.client.spec.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-tool/tests/search-card.client.spec.tsx) — A test under the owning area exercises or imports `toolviews`. A test under the owning area exercises or imports `useSessions`.
- [`packages/client/ui-tool/tests/terminal-card.client.spec.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-tool/tests/terminal-card.client.spec.tsx) — A test under the owning area exercises or imports `toolviews`. A test under the owning area exercises or imports `useSessions`.
- [`packages/client/ui-tool/tests/toolview-slot.client.spec.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-tool/tests/toolview-slot.client.spec.tsx) — A test under the owning area exercises or imports `entryKey`. A test under the owning area exercises or imports `toolview`.

## How to read the implementation

1. Start with [`packages/client/ui-tool/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-tool/src/index.ts) because it has the strongest evidence link to the note.
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
- **`rust/newtype-ids`** — Represent identity domains with Rust newtypes; do not pass bare strings across session, call, turn, step, and agent boundaries.
- **`rust/process-discipline`** — Treat this as repository or release policy unless it changes a runtime contract. Do not add runtime machinery for a process-only lesson.
- **`rust/provider-adapter`** — Keep provider wire quirks inside adapters; the loop and durable event vocabulary remain provider-neutral.
- **`rust/serde-validation`** — Parse untrusted input into strict serde types and validate semantic constraints before executing side effects.
- **`rust/tagged-enums`** — Use serde-tagged enums for a closed wire vocabulary. Add an explicit extension envelope only where runtime extension is truly required.
- **`rust/trait-seam`** — Use a small trait at the capability boundary; keep policy and model-facing schemas outside the backend implementation.

### Smallest safe implementation slice

1. Express the semantic contract as Rust types and one owning boundary.
2. Add the smallest adapter or service needed by the existing loop; do not add a new model-facing tool unless the four-tool surface cannot express the job.
3. Record durable evidence before deriving summaries or UI state.
4. Add one happy-path test, one interruption test, and one replay/restart test where state survives a turn.
5. Keep the patch reversible until the source note's strongest failure case is proven in the Rust harness.

## Search handles

- Tags: `class/architecture`, `concern/boundary`, `concern/compatibility`, `concern/discovery-routing`, `concern/evidence`, `concern/ownership`, `concern/performance`, `concern/recovery`, `concern/schema-types`, `domain/build-release`, `domain/llm`, `domain/session-state`, `lifecycle/implemented`, `mechanism/policy`, `mechanism/projection`, `mechanism/registry`
- Aliases: `inject`, `SlotMap`, `useSessions`, `entryKey`, `ctx.toolviews`, `conversation.chat.toolview`, `ui-tool`, `tool.call.toolview`, `renderToolView`, `registerToolView`, `ctx.slots.inject`, `Toolview dissolution --- tool rows are per-view keyed slots`, `architecture`, `boundary`
- Regex: `(?i)(inject|SlotMap|useSessions|entryKey|ctx\.toolviews|conversation\.chat\.toolview|ui\-tool|tool\.call\.toolview)`

```bash
rg -n --pcre2 "(?i)(inject|SlotMap|useSessions|entryKey|ctx\\.toolviews|conversation\\.chat\\.toolview|ui\\-tool|tool\\.call\\.toolview)" source-deepseek-harness
rg -l --fixed-strings "class/architecture" 03-agent-context
```

## Connected notes

- **`source-link`** — [0045. Web client architecture --- the client cordis plugin tree, the slot system, and the React-free object layer](0045-web-client-architecture-the-client-cordis-plugin-tree-the-slot-system-an.md): The source note links to this decision directly.
- **`source-link`** — [0052. The slot system standard --- single register, four props shares, and the framework store seat](0052-the-slot-system-standard-single-register-four-props-shares-and-the-frame.md): The source note links to this decision directly.
- **`source-link`** — [0111. Client Tool presentation ownership](0111-client-tool-presentation-ownership.md): The source note links to this decision directly.
- **`shares-code-with`** — [0283. Durable workflow runs in Chat](0283-durable-workflow-runs-in-chat.md): Shares source implementation: `packages/client/ui-tool/src/index.ts`, `packages/client/ui-tool/src/invariant.ts`.
- **`shares-code-with`** — [0044. GUI layering and the RPC protocol --- host/client layering by capability provider, the four-quadrant message model, and the fetch carrier](0044-gui-layering-and-the-rpc-protocol-host-client-layering-by-capability-pro.md): Shares source implementation: `packages/client/web/src/app.tsx`.
- **`shares-code-with`** — [0515. Semantic phases for composer-chain election](0515-semantic-phases-for-composer-chain-election.md): Shares source implementation: `packages/client/ui-tool/src/client/contract/slots.ts`.
- **`shares-code-with`** — [0185. Workspace UI Complete Product Flow](0185-workspace-ui-complete-product-flow.md): Shares source implementation: `packages/client/web/src/app.tsx`.
- **`shares-code-with`** — [0446. Drop the mutable session summary](0446-drop-the-mutable-session-summary.md): Shares source implementation: `packages/client/web/src/app.tsx`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0055-toolview-dissolution-tool-rows-are-per-view-keyed-slots.md`.
