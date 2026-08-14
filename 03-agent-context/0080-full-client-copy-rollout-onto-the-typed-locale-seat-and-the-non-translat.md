---
id: "dsh-note-0080"
title: "Full client copy rollout onto the typed locale seat, and the non-translation boundary"
status: "implemented"
class: "architecture"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/architecture/2026-07-30-client-locale-full-rollout.md"
implementation_evidence: "medium"
target_anchor: "read/write/patch filesystem capability"
tags:
  - "class/architecture"
  - "concern/boundary"
  - "concern/compatibility"
  - "concern/discovery-routing"
  - "concern/evidence"
  - "concern/lifecycle"
  - "concern/ownership"
  - "concern/performance"
  - "concern/recovery"
  - "concern/schema-types"
  - "domain/configuration"
  - "domain/extensions"
  - "domain/filesystem"
  - "domain/session-state"
  - "domain/shell-terminal"
  - "domain/storage"
  - "domain/testing"
  - "domain/ui-interaction"
  - "domain/web-retrieval"
  - "lifecycle/implemented"
  - "mechanism/generation"
  - "mechanism/projection"
  - "mechanism/registry"
aliases:
  - "codeLabels"
  - "ConnectionBanner"
  - "HoverCard"
  - "JsonTree"
  - "Modal"
  - "TerminalBlock"
  - "CodeBlock"
  - "JsonBlock"
  - "MarkdownText"
  - "SlotLabel"
  - "resolveSlotLabel"
  - "relativeTime"
  - "identity"
  - "blank"
search_regex: "(?i)(codeLabels|ConnectionBanner|HoverCard|JsonTree|Modal|TerminalBlock|CodeBlock|JsonBlock)"
---

# 0080. Full client copy rollout onto the typed locale seat, and the non-translation boundary — implementation context

## Open this when

After the typed locale standard seat landed (locale: on register → framework-injected typed t), only four early adopters rode it; every other client package still shipped hardcoded, mixed-language literals. Migrating the rest required mechanisms and boundary decisions the early adopters never touched: how registration-time text (nav rows, view-tab labels) refreshes on a language switch; how the zero-cordis ui-primitives atoms receive copy; and which strings deliberately stay untranslated --- an unrecorded boundary invites a future agent to "complete" the localization.

## Source decision

Registration-time text rides a label thunk. A list registration's label accepts SlotLabel = string | (() => string); owners projecting ledger rows resolve through resolveSlotLabel (never reading options.label raw) and make the read point follow the locale revision (outlets subscribe to the revision themselves; off-ledger projections such as the ui-settings nav fold the revision into their cache key and subscribe to both sources). Thunks evaluate per read, so a language switch causes zero ledger churn --- no re-registration, versions stay put, and every locale/change re-registration wiring is deleted.

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/architecture/2026-07-30-client-locale-full-rollout.md](../02-notes/implemented/architecture/2026-07-30-client-locale-full-rollout.md)
- Pinned source: [.agents/notes/implemented/architecture/2026-07-30-client-locale-full-rollout.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/architecture/2026-07-30-client-locale-full-rollout.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`packages/test-support/client-runtime/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/test-support/client-runtime/src/index.ts) | package entry point | Core file in the package named by the note: `packages/test-support/client-runtime`. | `named-package-member` |
| [`packages/test-support/client-runtime/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/test-support/client-runtime/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/test-support/client-runtime`. | `named-package-member` |
| [`packages/test-support/client-runtime/src/translate.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/test-support/client-runtime/src/translate.ts) | runtime implementation | Core file in the package named by the note: `packages/test-support/client-runtime`. Defines `makeTranslate`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/test-support/client-runtime`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/packages/test-support/client-runtime) | package or module directory | The note names this package or capability. | `named-package` |
| [`vendor/cordis/src/events.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/cordis/src/events.ts) | runtime implementation | Defines `label`, a construct named by the note. | `symbol-definition` |
| [`packages/fs/fs-local/src/fsio.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/fs-local/src/fsio.ts) | runtime implementation | Defines `identity`, a construct named by the note. | `symbol-definition` |
| [`packages/client/ui-slots/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-slots/src/index.ts) | package entry point | Defines `resolveSlotLabel`, a construct named by the note. Defines `SlotLabel`, a construct named by the note. | `symbol-definition` |
| [`packages/host/apiproxy/src/api-proxy.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/src/api-proxy.ts) | runtime implementation | Defines `labels`, a construct named by the note. Defines `blank`, a construct named by the note. | `symbol-definition` |
| [`packages/workspace/workspace/src/spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/workspace/workspace/src/spec.ts) | runtime implementation | Defines `workspaceId`, a construct named by the note. | `symbol-definition` |
| [`packages/client/ui-primitives/src/Modal.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-primitives/src/Modal.tsx) | runtime implementation | Defines `Modal`, a construct named by the note. | `symbol-definition` |
| [`packages/client/ui-primitives/src/JsonTree.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-primitives/src/JsonTree.tsx) | runtime implementation | Defines `JsonTree`, a construct named by the note. | `symbol-definition` |
| [`packages/client/ui-primitives/src/HoverCard.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-primitives/src/HoverCard.tsx) | runtime implementation | Defines `HoverCard`, a construct named by the note. | `symbol-definition` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `codeLabels` | `const` | [`packages/client/ui-conversation/src/client/chat/AssistantMarkdown.tsx:43`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/src/client/chat/AssistantMarkdown.tsx#L43) | `const codeLabels = useMemo(() => ({ copyLabel: t('copy'), copiedLabel: t('copied') }), [t])` |
| `ConnectionBanner` | `function` | [`packages/client/ui-primitives/src/ConnectionBanner.tsx:15`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-primitives/src/ConnectionBanner.tsx#L15) | `export function ConnectionBanner({ reconnecting, label = '连接已断开，正在重连…' }: {` |
| `HoverCard` | `function` | [`packages/client/ui-primitives/src/HoverCard.tsx:31`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-primitives/src/HoverCard.tsx#L31) | `export function HoverCard({` |
| `JsonTree` | `function` | [`packages/client/ui-primitives/src/JsonTree.tsx:407`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-primitives/src/JsonTree.tsx#L407) | `export function JsonTree({` |
| `Modal` | `function` | [`packages/client/ui-primitives/src/Modal.tsx:30`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-primitives/src/Modal.tsx#L30) | `export function Modal({` |
| `TerminalBlock` | `function` | [`packages/client/ui-primitives/src/TerminalBlock.tsx:174`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-primitives/src/TerminalBlock.tsx#L174) | `export function TerminalBlock({` |
| `CodeBlock` | `function` | [`packages/client/ui-primitives/src/markdown/CodeBlock.tsx:26`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-primitives/src/markdown/CodeBlock.tsx#L26) | `export function CodeBlock({ code, lang, className, copyLabel = '复制', copiedLabel = '复制成功' }: CodeBlockProps) {` |
| `JsonBlock` | `function` | [`packages/client/ui-primitives/src/markdown/JsonBlock.tsx:13`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-primitives/src/markdown/JsonBlock.tsx#L13) | `export function JsonBlock({ label, payload, defaultOpen = false, truncatedLabel = defaultTruncatedLabel }: {` |
| `MarkdownText` | `const` | [`packages/client/ui-primitives/src/markdown/MarkdownText.tsx:156`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-primitives/src/markdown/MarkdownText.tsx#L156) | `export const MarkdownText = memo(function MarkdownText({ text, streaming = false, codeLabels, fileMentions }: {` |
| `SlotLabel` | `type` | [`packages/client/ui-slots/src/index.ts:474`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-slots/src/index.ts#L474) | `export type SlotLabel = string \| (() => string)` |
| `resolveSlotLabel` | `function` | [`packages/client/ui-slots/src/index.ts:581`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-slots/src/index.ts#L581) | `export function resolveSlotLabel(label: SlotLabel \| undefined): string \| undefined {` |
| `relativeTime` | `function` | [`packages/client/ui-workspace/src/client/tree.ts:402`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-workspace/src/client/tree.ts#L402) | `export function relativeTime(updatedAt: number, now: number): RelativeTime {` |
| `identity` | `const` | [`packages/fs/fs-local/src/fsio.ts:270`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/fs-local/src/fsio.ts#L270) | `const identity = await resolveLocalTarget(parent.targetKey, name)` |
| `blank` | `const` | [`packages/host/apiproxy/src/api-proxy.ts:513`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/src/api-proxy.ts#L513) | `const blank = state.blank && event.type !== 'turn/start'` |
| `labels` | `const` | [`packages/host/apiproxy/src/api-proxy.ts:730`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/src/api-proxy.ts#L730) | `const labels = new Set(question.options?.map(option => option.label) ?? [])` |
| `makeTranslate` | `function` | [`packages/test-support/client-runtime/src/translate.ts:16`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/test-support/client-runtime/src/translate.ts#L16) | `export function makeTranslate(` |

### Tests and executable evidence

- [`packages/client/ui-workspace/tests/tree.client.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-workspace/tests/tree.client.spec.ts) — A test under the owning area exercises or imports `relativeTime`.
- [`packages/client/ui-primitives/tests/ansi.client.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-primitives/tests/ansi.client.spec.ts) — A test under the owning area exercises or imports `TerminalBlock`.
- [`packages/client/ui-primitives/tests/atoms.client.spec.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-primitives/tests/atoms.client.spec.tsx) — A test under the owning area exercises or imports `ConnectionBanner`. A test under the owning area exercises or imports `Modal`.
- [`packages/client/ui-primitives/tests/markdown.client.spec.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-primitives/tests/markdown.client.spec.tsx) — A test under the owning area exercises or imports `CodeBlock`. A test under the owning area exercises or imports `MarkdownText`.
- [`packages/client/ui-primitives/tests/json-tree.client.spec.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-primitives/tests/json-tree.client.spec.tsx) — A test under the owning area exercises or imports `JsonTree`.
- [`packages/client/ui-primitives/tests/hover-card.client.spec.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-primitives/tests/hover-card.client.spec.tsx) — A test under the owning area exercises or imports `HoverCard`.
- [`packages/client/ui-primitives/tests/code-block.client.spec.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-primitives/tests/code-block.client.spec.tsx) — A test under the owning area exercises or imports `CodeBlock`. A test under the owning area exercises or imports `MarkdownText`.
- [`packages/client/ui-primitives/tests/terminal-block.client.spec.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-primitives/tests/terminal-block.client.spec.tsx) — A test under the owning area exercises or imports `TerminalBlock`.

## How to read the implementation

1. Start with [`packages/test-support/client-runtime/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/test-support/client-runtime/src/index.ts) because it has the strongest evidence link to the note.
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
- **`rust/newtype-ids`** — Represent identity domains with Rust newtypes; do not pass bare strings across session, call, turn, step, and agent boundaries.
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

- Tags: `class/architecture`, `concern/boundary`, `concern/compatibility`, `concern/discovery-routing`, `concern/evidence`, `concern/lifecycle`, `concern/ownership`, `concern/performance`, `concern/recovery`, `concern/schema-types`, `domain/configuration`, `domain/extensions`, `domain/filesystem`, `domain/session-state`, `domain/shell-terminal`, `domain/storage`, `domain/testing`, `domain/ui-interaction`, `domain/web-retrieval`, `lifecycle/implemented`, `mechanism/generation`, `mechanism/projection`, `mechanism/registry`
- Aliases: `codeLabels`, `ConnectionBanner`, `HoverCard`, `JsonTree`, `Modal`, `TerminalBlock`, `CodeBlock`, `JsonBlock`, `MarkdownText`, `SlotLabel`, `resolveSlotLabel`, `relativeTime`, `identity`, `blank`
- Regex: `(?i)(codeLabels|ConnectionBanner|HoverCard|JsonTree|Modal|TerminalBlock|CodeBlock|JsonBlock)`

```bash
rg -n --pcre2 "(?i)(codeLabels|ConnectionBanner|HoverCard|JsonTree|Modal|TerminalBlock|CodeBlock|JsonBlock)" source-deepseek-harness
rg -l --fixed-strings "class/architecture" 03-agent-context
```

## Connected notes

- **`source-link`** — [0227. The Settings language a fresh browser opens in comes from the browser](0227-the-settings-language-a-fresh-browser-opens-in-comes-from-the-browser.md): The source note links to this decision directly.
- **`source-link`** — [0510. Client Settings, Locale, and Theme layering](0510-client-settings-locale-and-theme-layering.md): The source note links to this decision directly.
- **`shares-code-with`** — [0199. Workspace Registration Deletion](0199-workspace-registration-deletion.md): Shares source implementation: `packages/client/ui-primitives/src/Modal.tsx`, `packages/host/apiproxy/src/api-proxy.ts`.
- **`shares-code-with`** — [0185. Workspace UI Complete Product Flow](0185-workspace-ui-complete-product-flow.md): Shares source implementation: `packages/host/apiproxy/src/api-proxy.ts`, `packages/workspace/workspace/src/spec.ts`.
- **`shares-code-with`** — [0564. Bash-backed grep and glob discovery tools](0564-bash-backed-grep-and-glob-discovery-tools.md): Shares source implementation: `packages/fs/fs-local/src/fsio.ts`.
- **`shares-code-with`** — [0070. A capability-discriminated directory-picker seam for the web-GUI host](0070-a-capability-discriminated-directory-picker-seam-for-the-web-gui-host.md): Shares source implementation: `packages/host/apiproxy/src/api-proxy.ts`.
- **`shares-code-with`** — [0106. Subagent list identity via the projection unit](0106-subagent-list-identity-via-the-projection-unit.md): Shares source implementation: `vendor/cordis/src/events.ts`.
- **`shares-code-with`** — [0520. Task Surface for structured session interaction](0520-task-surface-for-structured-session-interaction.md): Shares source implementation: `packages/client/ui-slots/src/index.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0080-full-client-copy-rollout-onto-the-typed-locale-seat-and-the-non-translat.md`.
