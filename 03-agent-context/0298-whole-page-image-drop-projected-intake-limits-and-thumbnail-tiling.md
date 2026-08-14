---
id: "dsh-note-0298"
title: "Whole-page image drop, projected intake limits, and thumbnail tiling"
status: "implemented"
class: "feature"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/feature/2026-08-12-web-image-intake-and-limits-alignment.md"
implementation_evidence: "medium"
target_anchor: "Rust provider-adapter boundary"
tags:
  - "class/feature"
  - "concern/boundary"
  - "concern/evidence"
  - "concern/lifecycle"
  - "concern/ownership"
  - "concern/performance"
  - "domain/build-release"
  - "domain/configuration"
  - "domain/llm"
  - "domain/session-state"
  - "domain/testing"
  - "domain/ui-interaction"
  - "domain/web-retrieval"
  - "lifecycle/implemented"
  - "mechanism/projection"
aliases:
  - "image"
  - "inject"
  - "apply"
  - "ImageAttachmentRef"
  - "DropOverlay"
  - "imageLimits"
  - "intakeImages"
  - "IconCloseOutline16"
  - "disabled"
  - "attachments"
  - "describe"
  - "ImageBlock"
  - "SessionProjectionMap"
  - "maxImagesPerMessage"
search_regex: "(?i)(image|inject|apply|ImageAttachmentRef|DropOverlay|imageLimits|intakeImages|IconCloseOutline16)"
---

# 0298. Whole-page image drop, projected intake limits, and thumbnail tiling — implementation context

## Open this when

The second alignment step for issue #2248, after the attachment display note (whose rail/toast/atoms decisions stand; this note supersedes its history-gallery geometry and the lightbox backdrop specifics). Remaining gaps against DeepSeek Chat: images could only be dropped on the composer card --- a drop over the transcript navigated the browser away to the file; the lightbox close glyph was a bare × text character (buttons inherit no font family and the glyph's ink sits above the line box center, so it rendered visibly off-center) over a color-mix(label-primary 74%) backdrop that inverts to a bright white wash.

## Source decision

Whole-page drop. InputBar binds dragenter/dragover/dragleave/drop on the document (enter/leave depth counting, viewport-edge and dragend resets, Files-type gating so text drags keep their native textarea path) and renders the new DropOverlay atom in ui-attachment: a body-portaled, pointer-inert full-viewport layer (DeepSeek Chat's DragMask visuals --- white/70% + 10px blur, dark rgba(39,39,48,0.7), illustration, title, limits line) whose disabled variant announces a locked/busy composer.

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/feature/2026-08-12-web-image-intake-and-limits-alignment.md](../02-notes/implemented/feature/2026-08-12-web-image-intake-and-limits-alignment.md)
- Pinned source: [.agents/notes/implemented/feature/2026-08-12-web-image-intake-and-limits-alignment.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/feature/2026-08-12-web-image-intake-and-limits-alignment.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`packages/llm/llm/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/src/index.ts) | package entry point | Core file in the package named by the note: `packages/llm/llm`. | `named-package-member` |
| [`packages/llm/llm/src/types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/src/types.ts) | public types and contract | Core file in the package named by the note: `packages/llm/llm`. Defines `ImageBlock`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/llm/llm/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/llm/llm`. Defines `apply`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/core/session/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/index.ts) | package entry point | Core file in the package named by the note: `packages/core/session`. Defines `attachments`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/core/session/src/types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/types.ts) | public types and contract | Core file in the package named by the note: `packages/core/session`. | `named-package-member` |
| [`packages/core/session/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/core/session`. Defines `apply`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/client/ui-attachment/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-attachment/src/index.ts) | package entry point | Core file in the package named by the note: `packages/client/ui-attachment`. | `named-package-member` |
| [`packages/client/ui-primitives/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-primitives/src/index.ts) | package entry point | Core file in the package named by the note: `packages/client/ui-primitives`. | `named-package-member` |
| [`packages/attachment/attachment/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/attachment/attachment/src/index.ts) | package entry point | Core file in the package named by the note: `packages/attachment/attachment`. | `named-package-member` |
| [`packages/attachment/attachment/src/types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/attachment/attachment/src/types.ts) | public types and contract | Core file in the package named by the note: `packages/attachment/attachment`. Defines `ImageAttachmentRef`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/client/ui-attachment/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-attachment/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/client/ui-attachment`. Defines `apply`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/client/ui-primitives/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-primitives/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/client/ui-primitives`. Defines `apply`, a construct named by the note. | `named-package-member, symbol-definition` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `image` | `const` | [`packages/attachment/attachment-local/src/image.ts:55`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/attachment/attachment-local/src/image.ts#L55) | `const image = sharp(data, { failOn: 'error', limitInputPixels: false })` |
| `inject` | `const` | [`packages/attachment/attachment/src/invariant.ts:11`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/attachment/attachment/src/invariant.ts#L11) | `export const inject = ['invariants']` |
| `apply` | `const` | [`packages/attachment/attachment/src/invariant.ts:19`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/attachment/attachment/src/invariant.ts#L19) | `export const apply = (ctx: Context): Promise<() => void> => Promise.resolve(ctx.invariants.register(PACKAGE_NAME, install))` |
| `ImageAttachmentRef` | `interface` | [`packages/attachment/attachment/src/types.ts:11`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/attachment/attachment/src/types.ts#L11) | `export interface ImageAttachmentRef {` |
| `DropOverlay` | `function` | [`packages/client/ui-attachment/src/DropOverlay.tsx:24`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-attachment/src/DropOverlay.tsx#L24) | `export function DropOverlay({ disabled, labels }: {` |
| `inject` | `const` | [`packages/client/ui-attachment/src/invariant.ts:15`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-attachment/src/invariant.ts#L15) | `export const inject = ['invariants']` |
| `apply` | `const` | [`packages/client/ui-attachment/src/invariant.ts:29`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-attachment/src/invariant.ts#L29) | `export const apply = (ctx: Context): Promise<() => void> =>` |
| `imageLimits` | `const` | [`packages/client/ui-conversation/src/client/skeleton/InputBar.tsx:90`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/src/client/skeleton/InputBar.tsx#L90) | `const imageLimits = useProjection('imageLimits')` |
| `intakeImages` | `const` | [`packages/client/ui-conversation/src/client/skeleton/InputBar.tsx:424`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/src/client/skeleton/InputBar.tsx#L424) | `const intakeImages = useCallback((files: readonly File[]): void => {` |
| `IconCloseOutline16` | `const` | [`packages/client/ui-primitives/src/icons/index.tsx:211`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-primitives/src/icons/index.tsx#L211) | `export const IconCloseOutline16 = ({ size = 16, className }: IconProps) => (` |
| `inject` | `const` | [`packages/client/ui-primitives/src/invariant.ts:15`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-primitives/src/invariant.ts#L15) | `export const inject = ['invariants']` |
| `apply` | `const` | [`packages/client/ui-primitives/src/invariant.ts:29`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-primitives/src/invariant.ts#L29) | `export const apply = (ctx: Context): Promise<() => void> =>` |
| `disabled` | `const` | [`packages/client/ui-settings-plugins/src/client/BashCard.tsx:23`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-settings-plugins/src/client/BashCard.tsx#L23) | `const disabled = !state.writable` |
| `attachments` | `const` | [`packages/core/session/src/index.ts:415`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/index.ts#L415) | `const attachments = new WeakMap<Session, SessionEntry>()` |
| `inject` | `const` | [`packages/core/session/src/invariant.ts:20`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/invariant.ts#L20) | `export const inject = ['invariants']` |
| `apply` | `const` | [`packages/core/session/src/invariant.ts:249`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/invariant.ts#L249) | `export const apply = (ctx: Context): Promise<() => void> =>` |

### Tests and executable evidence

- [`packages/attachment/attachment-local/tests/index.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/attachment/attachment-local/tests/index.spec.ts) — A test under the owning area exercises or imports `maxImagesPerMessage`. A test under the owning area exercises or imports `imageLimits`.
- [`packages/attachment/attachment-local/tests/store.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/attachment/attachment-local/tests/store.spec.ts) — A test under the owning area exercises or imports `maxImagesPerMessage`.
- [`packages/session/session-projection/tests/registry.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/session/session-projection/tests/registry.spec.ts) — A test under the owning area exercises or imports `SessionProjectionMap`.
- [`packages/client/ui-conversation/tests/input-bar.client.spec.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/tests/input-bar.client.spec.tsx) — A test under the owning area exercises or imports `imageLimits`.
- [`packages/client/ui-attachment/tests/drop-overlay.client.spec.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-attachment/tests/drop-overlay.client.spec.tsx) — A test under the owning area exercises or imports `DropOverlay`.
- [`packages/client/ui-attachment/tests/message-image.client.spec.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-attachment/tests/message-image.client.spec.tsx) — A test under the owning area exercises or imports `dsh-attachment`.
- [`scripts/test-invariants.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/test-invariants.ts) — Contains the exact code literal `dsh-attachment` named by the note.
- [`scripts/client-bundle-purity.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/client-bundle-purity.spec.ts) — Contains the exact code literal `dsh-session` named by the note.

## How to read the implementation

1. Start with [`packages/llm/llm/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/src/index.ts) because it has the strongest evidence link to the note.
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
- **`rust/quickjs-boundary`** — QuickJS holds orchestration variables; Rust/Tokio owns async execution and exposes only explicitly granted capabilities.
- **`rust/trait-seam`** — Use a small trait at the capability boundary; keep policy and model-facing schemas outside the backend implementation.

### Smallest safe implementation slice

1. Express the semantic contract as Rust types and one owning boundary.
2. Add the smallest adapter or service needed by the existing loop; do not add a new model-facing tool unless the four-tool surface cannot express the job.
3. Record durable evidence before deriving summaries or UI state.
4. Add one happy-path test, one interruption test, and one replay/restart test where state survives a turn.
5. Keep the patch reversible until the source note's strongest failure case is proven in the Rust harness.

## Search handles

- Tags: `class/feature`, `concern/boundary`, `concern/evidence`, `concern/lifecycle`, `concern/ownership`, `concern/performance`, `domain/build-release`, `domain/configuration`, `domain/llm`, `domain/session-state`, `domain/testing`, `domain/ui-interaction`, `domain/web-retrieval`, `lifecycle/implemented`, `mechanism/projection`
- Aliases: `image`, `inject`, `apply`, `ImageAttachmentRef`, `DropOverlay`, `imageLimits`, `intakeImages`, `IconCloseOutline16`, `disabled`, `attachments`, `describe`, `ImageBlock`, `SessionProjectionMap`, `maxImagesPerMessage`
- Regex: `(?i)(image|inject|apply|ImageAttachmentRef|DropOverlay|imageLimits|intakeImages|IconCloseOutline16)`

```bash
rg -n --pcre2 "(?i)(image|inject|apply|ImageAttachmentRef|DropOverlay|imageLimits|intakeImages|IconCloseOutline16)" source-deepseek-harness
rg -l --fixed-strings "class/feature" 03-agent-context
```

## Connected notes

- **`source-link`** — [0294. Web attachment display aligns with DeepSeek Chat via attachment atoms](0294-web-attachment-display-aligns-with-deepseek-chat-via-attachment-atoms.md): The source note links to this decision directly.
- **`shares-code-with`** — [0025. Every LLM request is reconstructable from the session log](0025-every-llm-request-is-reconstructable-from-the-session-log.md): Shares source implementation: `packages/core/session/src/index.ts`, `packages/core/session/src/invariant.ts`.
- **`shares-code-with`** — [0522. Architectural conformance --- dependency rules and the adapter kit](0522-architectural-conformance-dependency-rules-and-the-adapter-kit.md): Shares source implementation: `packages/core/session/src/index.ts`, `packages/core/session/src/invariant.ts`.
- **`shares-code-with`** — [0016. Bounded recovery for transient LLM request failures](0016-bounded-recovery-for-transient-llm-request-failures.md): Shares source implementation: `packages/core/session/src/index.ts`, `packages/core/session/src/types.ts`.
- **`shares-code-with`** — [0498. Per-session snapshot replay for nested agents](0498-per-session-snapshot-replay-for-nested-agents.md): Shares source implementation: `packages/core/session/src/index.ts`, `packages/core/session/src/invariant.ts`.
- **`shares-code-with`** — [0133. Compaction as a capability seam (abstract contract + basic backend)](0133-compaction-as-a-capability-seam-abstract-contract-basic-backend.md): Shares source implementation: `packages/core/session/src/index.ts`, `packages/core/session/src/types.ts`.
- **`shares-code-with`** — [0516. Migrate simple unary API Proxy calls to business Remote services](0516-migrate-simple-unary-api-proxy-calls-to-business-remote-services.md): Shares source implementation: `packages/core/session/src/index.ts`, `packages/core/session/src/types.ts`.
- **`shares-code-with`** — [0326. The browser conversation is a log-ordered human transcript](0326-the-browser-conversation-is-a-log-ordered-human-transcript.md): Shares source implementation: `packages/core/session/src/index.ts`, `packages/core/session/src/types.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0298-whole-page-image-drop-projected-intake-limits-and-thumbnail-tiling.md`.
