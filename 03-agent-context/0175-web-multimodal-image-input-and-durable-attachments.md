---
id: "dsh-note-0175"
title: "Web multimodal image input and durable attachments"
status: "implemented"
class: "feature"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/feature/2026-07-22-web-multimodal-image-input-and-durable-attachments.md"
implementation_evidence: "high"
target_anchor: "read/write/patch filesystem capability"
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
  - "concern/performance"
  - "concern/recovery"
  - "concern/schema-types"
  - "concern/simplification"
  - "concern/trust"
  - "domain/build-release"
  - "domain/context"
  - "domain/extensions"
  - "domain/filesystem"
  - "domain/llm"
  - "domain/protocols"
  - "domain/session-state"
  - "domain/shell-terminal"
  - "domain/storage"
  - "domain/testing"
  - "domain/ui-interaction"
  - "domain/web-retrieval"
  - "lifecycle/implemented"
  - "mechanism/adapter"
  - "mechanism/event-sourcing"
  - "mechanism/policy"
  - "mechanism/registry"
aliases:
  - "image"
  - "ImageAttachmentRef"
  - "maxRequestBodyBytes"
  - "conversation"
  - "ImageLightbox"
  - "MessageImage"
  - "InputMachine"
  - "ConversationController"
  - "InputBar"
  - "attachments"
  - "dshHome"
  - "attachment"
  - "ImageBlock"
  - "ContentBlockMap"
search_regex: "(?i)(image|ImageAttachmentRef|maxRequestBodyBytes|conversation|ImageLightbox|MessageImage|InputMachine|ConversationController)"
---

# 0175. Web multimodal image input and durable attachments — implementation context

## Open this when

Before this change, the Web composer accepted only text: InputBar received a string draft, ConversationController.send() created text content, and the host forwarded that content to the agent. Users could not paste an image, inspect it before sending, submit an image-only prompt, or recover sent images from history. This is not only a composer gap. Core needs a durable image content block, providers need explicit modality handling, and the session log must reconstruct everything visible to a model. The previous image-block removal rejected a partial design that could silently lose or flatten images.

## Source decision

Pasted or dropped raster images are the Web composer's first consumer of a durable attachment capability. Unsent files remain temporary client-owned draft state. The host validates and durably commits every accepted user image before appending its message event. A provider adapter that produces structured image output must durably commit the output before appending its assistant block. Canonical user and assistant content contains only role-neutral ImageBlock references.

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/feature/2026-07-22-web-multimodal-image-input-and-durable-attachments.md](../02-notes/implemented/feature/2026-07-22-web-multimodal-image-input-and-durable-attachments.md)
- Pinned source: [.agents/notes/implemented/feature/2026-07-22-web-multimodal-image-input-and-durable-attachments.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/feature/2026-07-22-web-multimodal-image-input-and-durable-attachments.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`packages/util/brand/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/util/brand/src/index.ts) | package entry point | Core file in the package named by the note: `packages/util/brand`. | `named-package-member` |
| [`packages/util/brand/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/util/brand/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/util/brand`. | `named-package-member` |
| [`packages/core/agent-loop/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent-loop/src/index.ts) | package entry point | Core file in the package named by the note: `packages/core/agent-loop`. | `named-package-member` |
| [`packages/core/agent-loop/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent-loop/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/core/agent-loop`. | `named-package-member` |
| [`packages/attachment/attachment/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/attachment/attachment/src/index.ts) | package entry point | Entry point or contract under the directory named by the note: `packages/attachment/attachment`. Core file in the package named by the note: `packages/attachment/attachment`. | `named-directory-member, named-package-member` |
| [`packages/attachment/attachment/src/types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/attachment/attachment/src/types.ts) | public types and contract | Entry point or contract under the directory named by the note: `packages/attachment/attachment`. Core file in the package named by the note: `packages/attachment/attachment`. | `named-directory-member, named-package-member, symbol-definition` |
| [`packages/attachment/attachment/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/attachment/attachment/src/invariant.ts) | runtime contract checks | Entry point or contract under the directory named by the note: `packages/attachment/attachment`. Core file in the package named by the note: `packages/attachment/attachment`. | `named-directory-member, named-package-member` |
| [`packages/compaction/compaction-basic/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/compaction/compaction-basic/src/index.ts) | package entry point | Entry point or contract under the directory named by the note: `packages/compaction/compaction-basic`. Core file in the package named by the note: `packages/compaction/compaction-basic`. | `named-directory-member, named-package-member` |
| [`packages/compaction/compaction-basic/src/types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/compaction/compaction-basic/src/types.ts) | public types and contract | Entry point or contract under the directory named by the note: `packages/compaction/compaction-basic`. Core file in the package named by the note: `packages/compaction/compaction-basic`. | `named-directory-member, named-package-member` |
| [`packages/compaction/compaction-basic/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/compaction/compaction-basic/src/invariant.ts) | runtime contract checks | Entry point or contract under the directory named by the note: `packages/compaction/compaction-basic`. Core file in the package named by the note: `packages/compaction/compaction-basic`. | `named-directory-member, named-package-member` |
| [`packages/acp/acp/README.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/acp/acp/README.md) | package contract and examples | Entry point or contract under the directory named by the note: `packages/acp/acp`. | `named-directory-member` |
| [`packages/llm/llm/README.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/README.md) | package contract and examples | Entry point or contract under the directory named by the note: `packages/llm/llm`. | `named-directory-member` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `image` | `const` | [`packages/attachment/attachment-local/src/image.ts:55`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/attachment/attachment-local/src/image.ts#L55) | `const image = sharp(data, { failOn: 'error', limitInputPixels: false })` |
| `ImageAttachmentRef` | `interface` | [`packages/attachment/attachment/src/types.ts:11`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/attachment/attachment/src/types.ts#L11) | `export interface ImageAttachmentRef {` |
| `maxRequestBodyBytes` | `const` | [`packages/client/connection/src/index.ts:133`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/connection/src/index.ts#L133) | `const maxRequestBodyBytes = config?.maxRequestBodyBytes ?? DEFAULT_MAX_REQUEST_BODY_BYTES` |
| `conversation` | `const` | [`packages/client/runtime/src/client/index.ts:190`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/runtime/src/client/index.ts#L190) | `const conversation = {` |
| `ImageLightbox` | `function` | [`packages/client/ui-attachment/src/ImageLightbox.tsx:27`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-attachment/src/ImageLightbox.tsx#L27) | `export function ImageLightbox({ src, alt, labels, onClose }: {` |
| `MessageImage` | `function` | [`packages/client/ui-attachment/src/MessageImage.tsx:54`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-attachment/src/MessageImage.tsx#L54) | `export function MessageImage({ attachment, load, variant, labels }: {` |
| `InputMachine` | `class` | [`packages/client/ui-conversation/src/client/input/machine.ts:105`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/src/client/input/machine.ts#L105) | `export class InputMachine {` |
| `ConversationController` | `class` | [`packages/client/ui-conversation/src/client/service.ts:91`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/src/client/service.ts#L91) | `export class ConversationController extends Service implements IConversation {` |
| `InputBar` | `function` | [`packages/client/ui-conversation/src/client/skeleton/InputBar.tsx:46`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/src/client/skeleton/InputBar.tsx#L46) | `export function InputBar({` |
| `attachments` | `const` | [`packages/core/session/src/index.ts:415`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/index.ts#L415) | `const attachments = new WeakMap<Session, SessionEntry>()` |
| `dshHome` | `const` | [`packages/examples/agent-spine-demo/src/index.ts:218`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/examples/agent-spine-demo/src/index.ts#L218) | `const dshHome = resolveDshHome(config.dshHome ?? nestedDshHome)` |
| `attachment` | `const` | [`packages/host/apiproxy/src/api-proxy.ts:180`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/src/api-proxy.ts#L180) | `const attachment = await ctx.attachments.saveImage({` |
| `ImageBlock` | `interface` | [`packages/llm/llm/src/types.ts:71`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/src/types.ts#L71) | `export interface ImageBlock {` |
| `ContentBlockMap` | `interface` | [`packages/llm/llm/src/types.ts:99`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/src/types.ts#L99) | `export interface ContentBlockMap {` |
| `prompt` | `const` | [`packages/schedule/schedule/src/domain.ts:389`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/schedule/schedule/src/domain.ts#L389) | `const prompt = value['prompt']` |

### Tests and executable evidence

- [`apps/web/tests/image-display.snapshot.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/image-display.snapshot.ts) — The source note names this file directly.
- [`packages/client/connection/tests/node-half.host.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/connection/tests/node-half.host.spec.ts) — A test under the owning area exercises or imports `maxRequestBodyBytes`.
- [`packages/client/ui-conversation/tests/skeleton.client.spec.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/tests/skeleton.client.spec.tsx) — A test under the owning area exercises or imports `InputBar`.
- [`packages/client/ui-conversation/tests/input-bar.client.spec.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/tests/input-bar.client.spec.tsx) — A test under the owning area exercises or imports `InputBar`.
- [`packages/client/ui-attachment/tests/message-image.client.spec.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-attachment/tests/message-image.client.spec.tsx) — A test under the owning area exercises or imports `MessageImage`.
- [`packages/client/ui-conversation/tests/input-matrix.client.spec.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/tests/input-matrix.client.spec.tsx) — A test under the owning area exercises or imports `InputBar`.
- [`packages/client/ui-conversation/tests/input-machine.client.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/tests/input-machine.client.spec.ts) — A test under the owning area exercises or imports `InputMachine`.
- [`packages/client/ui-attachment/tests/image-lightbox.client.spec.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-attachment/tests/image-lightbox.client.spec.tsx) — A test under the owning area exercises or imports `ImageLightbox`.
- Source verification intent: Storage tests cover content-addressed deduplication, private permissions, admission failures, corruption/missing-object failures, and reading history after deployment limits are lowered. Host and protocol tests cover persist-before-event ordering, absence of base64 in logs, session-scoped authorization, capability rejection, upload limits, bounded HTTP request bodies, image-admission/model-selection races (queued and steering placements), pending publication, idle release without publication, text-only queue edits, and selection against current derived history after compaction.

## How to read the implementation

1. Start with [`packages/util/brand/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/util/brand/src/index.ts) because it has the strongest evidence link to the note.
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

- Tags: `class/feature`, `concern/boundary`, `concern/compatibility`, `concern/concurrency`, `concern/discovery-routing`, `concern/evidence`, `concern/human-control`, `concern/lifecycle`, `concern/ownership`, `concern/performance`, `concern/recovery`, `concern/schema-types`, `concern/simplification`, `concern/trust`, `domain/build-release`, `domain/context`, `domain/extensions`, `domain/filesystem`, `domain/llm`, `domain/protocols`, `domain/session-state`, `domain/shell-terminal`, `domain/storage`, `domain/testing`, `domain/ui-interaction`, `domain/web-retrieval`, `lifecycle/implemented`, `mechanism/adapter`, `mechanism/event-sourcing`, `mechanism/policy`, `mechanism/registry`
- Aliases: `image`, `ImageAttachmentRef`, `maxRequestBodyBytes`, `conversation`, `ImageLightbox`, `MessageImage`, `InputMachine`, `ConversationController`, `InputBar`, `attachments`, `dshHome`, `attachment`, `ImageBlock`, `ContentBlockMap`
- Regex: `(?i)(image|ImageAttachmentRef|maxRequestBodyBytes|conversation|ImageLightbox|MessageImage|InputMachine|ConversationController)`

```bash
rg -n --pcre2 "(?i)(image|ImageAttachmentRef|maxRequestBodyBytes|conversation|ImageLightbox|MessageImage|InputMachine|ConversationController)" source-deepseek-harness
rg -l --fixed-strings "class/feature" 03-agent-context
```

## Connected notes

- **`source-link`** — [0044. GUI layering and the RPC protocol --- host/client layering by capability provider, the four-quadrant message model, and the fetch carrier](0044-gui-layering-and-the-rpc-protocol-host-client-layering-by-capability-pro.md): The source note links to this decision directly.
- **`source-link`** — [0045. Web client architecture --- the client cordis plugin tree, the slot system, and the React-free object layer](0045-web-client-architecture-the-client-cordis-plugin-tree-the-slot-system-an.md): The source note links to this decision directly.
- **`source-link`** — [0315. Atomic Web image admission](0315-atomic-web-image-admission.md): The source note links to this decision directly.
- **`source-link`** — [0294. Web attachment display aligns with DeepSeek Chat via attachment atoms](0294-web-attachment-display-aligns-with-deepseek-chat-via-attachment-atoms.md): The source note links to this decision directly.
- **`shares-code-with`** — [0520. Task Surface for structured session interaction](0520-task-surface-for-structured-session-interaction.md): Shares source implementation: `packages/util/brand/src/index.ts`, `packages/util/brand/src/invariant.ts`.
- **`shares-code-with`** — [0215. Queued manual compaction with one durable lock](0215-queued-manual-compaction-with-one-durable-lock.md): Shares source implementation: `packages/compaction/compaction-basic/src/index.ts`, `packages/compaction/compaction-basic/src/types.ts`.
- **`shares-code-with`** — [0033. After-call compaction pressure and context-overflow recovery](0033-after-call-compaction-pressure-and-context-overflow-recovery.md): Shares source implementation: `packages/compaction/compaction-basic/src/index.ts`, `packages/compaction/compaction-basic/src/types.ts`.
- **`shares-code-with`** — [0146. The subprocess sandbox --- confinement seam, native runners, escalation, and per-session modes](0146-the-subprocess-sandbox-confinement-seam-native-runners-escalation-and-pe.md): Shares source implementation: `packages/core/agent-loop/src/index.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0175-web-multimodal-image-input-and-durable-attachments.md`.
