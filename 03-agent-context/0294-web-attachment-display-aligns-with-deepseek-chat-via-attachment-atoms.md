---
id: "dsh-note-0294"
title: "Web attachment display aligns with DeepSeek Chat via attachment atoms"
status: "implemented"
class: "feature"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/feature/2026-08-11-web-attachment-display-alignment.md"
implementation_evidence: "medium"
target_anchor: "Rust provider-adapter boundary"
tags:
  - "class/feature"
  - "concern/boundary"
  - "concern/evidence"
  - "concern/lifecycle"
  - "concern/ownership"
  - "concern/recovery"
  - "concern/simplification"
  - "domain/extensions"
  - "domain/llm"
  - "domain/session-state"
  - "domain/storage"
  - "domain/ui-interaction"
  - "lifecycle/implemented"
aliases:
  - "AttachmentRail"
  - "ImageLightbox"
  - "MessageImage"
  - "ImageGallery"
  - "conversation"
  - "InputBar"
  - "promptError"
  - "ModelSelect"
  - "Toast"
  - "onDone"
  - "top/right: -6px"
  - "overflow-x"
  - "attachment-error"
  - "dsh-client-ui-conversation"
search_regex: "(?i)(AttachmentRail|ImageLightbox|MessageImage|ImageGallery|conversation|InputBar|promptError|ModelSelect)"
---

# 0294. Web attachment display aligns with DeepSeek Chat via attachment atoms — implementation context

## Open this when

The web composer's image surfaces missed basic usability (user feedback, issue #2248). The remove control hung outside each 72px thumbnail at top/right: -6px, so the rail's overflow-x box clipped it and clicks aimed at it often missed; previews opened only on double-click, an affordance nothing advertised except a tooltip; a rail wider than the composer produced a raw horizontal scrollbar inside the capsule; and image-intake rejections plus prompt failures (for example attachment-error when the selected model takes no image input) rendered as persistent inline red strips above the card.

## Source decision

Attachment display lives in a new zero-cordis atoms package, @deepseek-ai/dsh-client-ui-attachment (packages/client/ui-attachment), patterned on dsh-client-ui-primitives: AttachmentRail (64px/16px-radius thumbnails, single-click onOpen, inside-the-card remove control revealed on hover or focus and permanent under pointer: coarse, hidden scrollbar with circular edge arrows recomputed from scroll geometry, vertical-wheel horizontal pan clamped to 60px/tick, end-reveal on growth), MessageImage/ImageGallery (single-click preview), and ImageLightbox.

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/feature/2026-08-11-web-attachment-display-alignment.md](../02-notes/implemented/feature/2026-08-11-web-attachment-display-alignment.md)
- Pinned source: [.agents/notes/implemented/feature/2026-08-11-web-attachment-display-alignment.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/feature/2026-08-11-web-attachment-display-alignment.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`packages/client/ui-attachment/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-attachment/src/index.ts) | package entry point | Entry point or contract under the directory named by the note: `packages/client/ui-attachment`. Core file in the package named by the note: `packages/client/ui-attachment`. | `named-directory-member, named-package-member` |
| [`packages/client/ui-primitives/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-primitives/src/index.ts) | package entry point | Core file in the package named by the note: `packages/client/ui-primitives`. | `named-package-member` |
| [`packages/client/ui-primitives/src/Toast.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-primitives/src/Toast.tsx) | runtime implementation | Core file in the package named by the note: `packages/client/ui-primitives`. Defines `Toast`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/client/ui-conversation/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/src/index.ts) | package entry point | Core file in the package named by the note: `packages/client/ui-conversation`. | `named-package-member` |
| [`packages/client/ui-attachment/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-attachment/src/invariant.ts) | runtime contract checks | Entry point or contract under the directory named by the note: `packages/client/ui-attachment`. Core file in the package named by the note: `packages/client/ui-attachment`. | `named-directory-member, named-package-member` |
| [`packages/client/ui-primitives/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-primitives/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/client/ui-primitives`. | `named-package-member` |
| [`packages/client/ui-conversation/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/client/ui-conversation`. | `named-package-member` |
| [`packages/client/ui-attachment/src/MessageImage.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-attachment/src/MessageImage.tsx) | runtime implementation | Entry point or contract under the directory named by the note: `packages/client/ui-attachment`. Core file in the package named by the note: `packages/client/ui-attachment`. | `named-directory-member, named-package-member, symbol-definition` |
| [`packages/client/ui-attachment/src/ImageLightbox.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-attachment/src/ImageLightbox.tsx) | runtime implementation | Entry point or contract under the directory named by the note: `packages/client/ui-attachment`. Core file in the package named by the note: `packages/client/ui-attachment`. | `named-directory-member, named-package-member, symbol-definition` |
| [`packages/client/ui-conversation/src/client/apply.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/src/client/apply.ts) | runtime implementation | Core file in the package named by the note: `packages/client/ui-conversation`. Defines `conversation`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/client/ui-attachment/src/AttachmentRail.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-attachment/src/AttachmentRail.tsx) | runtime implementation | Entry point or contract under the directory named by the note: `packages/client/ui-attachment`. Core file in the package named by the note: `packages/client/ui-attachment`. | `named-directory-member, named-package-member, symbol-definition` |
| [`packages/client/ui-conversation/src/client/input/hub.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/src/client/input/hub.ts) | runtime implementation | Core file in the package named by the note: `packages/client/ui-conversation`. | `named-package-member` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `AttachmentRail` | `function` | [`packages/client/ui-attachment/src/AttachmentRail.tsx:67`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-attachment/src/AttachmentRail.tsx#L67) | `export function AttachmentRail<T extends AttachmentRailItem>({ items, labels, onOpen, onRemove }: {` |
| `ImageLightbox` | `function` | [`packages/client/ui-attachment/src/ImageLightbox.tsx:27`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-attachment/src/ImageLightbox.tsx#L27) | `export function ImageLightbox({ src, alt, labels, onClose }: {` |
| `MessageImage` | `function` | [`packages/client/ui-attachment/src/MessageImage.tsx:54`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-attachment/src/MessageImage.tsx#L54) | `export function MessageImage({ attachment, load, variant, labels }: {` |
| `ImageGallery` | `function` | [`packages/client/ui-attachment/src/MessageImage.tsx:105`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-attachment/src/MessageImage.tsx#L105) | `export function ImageGallery({ images, load, align, labels }: {` |
| `conversation` | `const` | [`packages/client/ui-conversation/src/client/apply.ts:95`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/src/client/apply.ts#L95) | `const conversation = scoped.get('conversation')` |
| `conversation` | `const` | [`packages/client/ui-conversation/src/client/apply.ts:102`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/src/client/apply.ts#L102) | `const conversation = ctx.get('conversation') as ConversationController \| undefined` |
| `conversation` | `const` | [`packages/client/ui-conversation/src/client/apply.ts:246`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/src/client/apply.ts#L246) | `const conversation = concreteConversation(ctx)` |
| `conversation` | `const` | [`packages/client/ui-conversation/src/client/apply.ts:303`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/src/client/apply.ts#L303) | `const conversation = concreteConversation(ctx)` |
| `conversation` | `const` | [`packages/client/ui-conversation/src/client/apply.ts:387`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/src/client/apply.ts#L387) | `const conversation = concreteConversation(ctx)` |
| `InputBar` | `function` | [`packages/client/ui-conversation/src/client/skeleton/InputBar.tsx:46`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/src/client/skeleton/InputBar.tsx#L46) | `export function InputBar({` |
| `promptError` | `const` | [`packages/client/ui-conversation/src/client/skeleton/InputBar.tsx:58`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/src/client/skeleton/InputBar.tsx#L58) | `const promptError = useSession(s => s.promptError) ?? null` |
| `ModelSelect` | `function` | [`packages/client/ui-model-selection/src/client/ModelSelect.tsx:45`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-model-selection/src/client/ModelSelect.tsx#L45) | `export function ModelSelect(` |
| `Toast` | `function` | [`packages/client/ui-primitives/src/Toast.tsx:28`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-primitives/src/Toast.tsx#L28) | `export function Toast({ text, icon, anchor, onDone }: {` |
| `onDone` | `const` | [`packages/code-runtime/code-runtime-worker-thread/src/index.ts:443`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/code-runtime/code-runtime-worker-thread/src/index.ts#L443) | `const onDone = (message: WorkerToHost): void => {` |

### Tests and executable evidence

- [`packages/client/ui-primitives/tests/toast.client.spec.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-primitives/tests/toast.client.spec.tsx) — A test under the owning area exercises or imports `Toast`. A test under the owning area exercises or imports `onDone`.
- [`packages/client/ui-conversation/tests/skeleton.client.spec.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/tests/skeleton.client.spec.tsx) — A test under the owning area exercises or imports `InputBar`. A test under the owning area exercises or imports `addImages`.
- [`packages/client/ui-conversation/tests/input-bar.client.spec.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/tests/input-bar.client.spec.tsx) — A test under the owning area exercises or imports `attachment-error`. A test under the owning area exercises or imports `InputBar`.
- [`packages/client/ui-conversation/tests/chat-view.client.spec.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/tests/chat-view.client.spec.tsx) — A test under the owning area exercises or imports `addImages`. A test under the owning area exercises or imports `promptError`.
- [`packages/client/ui-conversation/tests/queue-dock.client.spec.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/tests/queue-dock.client.spec.tsx) — A test under the owning area exercises or imports `promptError`.
- [`packages/client/ui-conversation/tests/chat-stats.client.spec.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/tests/chat-stats.client.spec.tsx) — A test under the owning area exercises or imports `promptError`.
- [`packages/client/ui-attachment/tests/message-image.client.spec.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-attachment/tests/message-image.client.spec.tsx) — A test under the owning area exercises or imports `MessageImage`. A test under the owning area exercises or imports `ImageGallery`.
- [`packages/client/ui-conversation/tests/input-matrix.client.spec.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/tests/input-matrix.client.spec.tsx) — A test under the owning area exercises or imports `InputBar`. A test under the owning area exercises or imports `addImages`.

## How to read the implementation

1. Start with [`packages/client/ui-attachment/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-attachment/src/index.ts) because it has the strongest evidence link to the note.
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

### Smallest safe implementation slice

1. Express the semantic contract as Rust types and one owning boundary.
2. Add the smallest adapter or service needed by the existing loop; do not add a new model-facing tool unless the four-tool surface cannot express the job.
3. Record durable evidence before deriving summaries or UI state.
4. Add one happy-path test, one interruption test, and one replay/restart test where state survives a turn.
5. Keep the patch reversible until the source note's strongest failure case is proven in the Rust harness.

## Search handles

- Tags: `class/feature`, `concern/boundary`, `concern/evidence`, `concern/lifecycle`, `concern/ownership`, `concern/recovery`, `concern/simplification`, `domain/extensions`, `domain/llm`, `domain/session-state`, `domain/storage`, `domain/ui-interaction`, `lifecycle/implemented`
- Aliases: `AttachmentRail`, `ImageLightbox`, `MessageImage`, `ImageGallery`, `conversation`, `InputBar`, `promptError`, `ModelSelect`, `Toast`, `onDone`, `top/right: -6px`, `overflow-x`, `attachment-error`, `dsh-client-ui-conversation`
- Regex: `(?i)(AttachmentRail|ImageLightbox|MessageImage|ImageGallery|conversation|InputBar|promptError|ModelSelect)`

```bash
rg -n --pcre2 "(?i)(AttachmentRail|ImageLightbox|MessageImage|ImageGallery|conversation|InputBar|promptError|ModelSelect)" source-deepseek-harness
rg -l --fixed-strings "class/feature" 03-agent-context
```

## Connected notes

- **`source-link`** — [0175. Web multimodal image input and durable attachments](0175-web-multimodal-image-input-and-durable-attachments.md): The source note links to this decision directly.
- **`shares-code-with`** — [0208. Ask-question Web presentation](0208-ask-question-web-presentation.md): Shares source implementation: `packages/client/ui-conversation/src/index.ts`, `packages/client/ui-conversation/src/invariant.ts`.
- **`shares-code-with`** — [0177. Safe assistant Markdown in the Web conversation](0177-safe-assistant-markdown-in-the-web-conversation.md): Shares source implementation: `packages/client/ui-conversation/src/index.ts`, `packages/client/ui-conversation/src/invariant.ts`.
- **`shares-code-with`** — [0520. Task Surface for structured session interaction](0520-task-surface-for-structured-session-interaction.md): Shares source implementation: `packages/client/ui-conversation/src/index.ts`, `packages/client/ui-conversation/src/invariant.ts`.
- **`shares-code-with`** — [0253. Web turn and window latency/throughput metrics](0253-web-turn-and-window-latency-throughput-metrics.md): Shares source implementation: `packages/client/ui-conversation/src/client/apply.ts`, `packages/client/ui-conversation/src/index.ts`.
- **`shares-code-with`** — [0220. Web read card frontend --- the read tool's line window renders line-numbered and highlighted](0220-web-read-card-frontend-the-read-tool-s-line-window-renders-line-numbered.md): Shares source implementation: `packages/client/ui-primitives/src/index.ts`, `packages/client/ui-primitives/src/invariant.ts`.
- **`shares-code-with`** — [0292. Web surface for message feedback](0292-web-surface-for-message-feedback.md): Shares source implementation: `packages/client/ui-conversation/src/index.ts`, `packages/client/ui-conversation/src/invariant.ts`.
- **`shares-code-with`** — [0225. Web search card --- the grep and glob render intent reaches the browser](0225-web-search-card-the-grep-and-glob-render-intent-reaches-the-browser.md): Shares source implementation: `packages/client/ui-primitives/src/index.ts`, `packages/client/ui-primitives/src/invariant.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0294-web-attachment-display-aligns-with-deepseek-chat-via-attachment-atoms.md`.
