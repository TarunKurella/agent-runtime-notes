---
id: "dsh-note-0468"
title: "Simplify Web image input version one"
status: "implemented"
class: "simplification"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/simplification/2026-07-29-simplify-web-image-input-v1.md"
implementation_evidence: "medium"
target_anchor: "read/write/patch filesystem capability"
tags:
  - "class/simplification"
  - "concern/boundary"
  - "concern/cancellation-timeout"
  - "concern/compatibility"
  - "concern/discovery-routing"
  - "concern/evidence"
  - "concern/lifecycle"
  - "concern/ownership"
  - "concern/schema-types"
  - "domain/build-release"
  - "domain/configuration"
  - "domain/extensions"
  - "domain/filesystem"
  - "domain/llm"
  - "domain/protocols"
  - "domain/session-state"
  - "domain/storage"
  - "domain/testing"
  - "domain/web-retrieval"
  - "lifecycle/implemented"
  - "mechanism/adapter"
  - "mechanism/event-sourcing"
  - "mechanism/policy"
  - "mechanism/registry"
aliases:
  - "IConversation"
  - "describe"
  - "ImageBlock"
  - "host.describe"
  - "validateImage"
  - "saveImage"
  - "readImage"
  - "Simplify Web image input version one"
  - "simplification"
  - "boundary"
  - "cancellation timeout"
  - "compatibility"
  - "discovery routing"
  - "evidence"
search_regex: "(?i)(IConversation|describe|ImageBlock|host\\.describe|validateImage|saveImage|readImage|Simplify[- ]Web[- ]image[- ]input[- ]version[- ]one)"
---

# 0468. Simplify Web image input version one — implementation context

## Open this when

The first durable Web image-input slice introduced required ordered multi-image intake alongside speculative surfaces for arbitrary CLI provider mounting, output-modality discovery, alternative text, provider-neutral visual token pricing, and browser lifecycle APIs with no cross-package consumer. Keeping the speculative surfaces would turn unchosen future behavior into public contracts and make the initial capability harder to review and maintain.

## Source decision

Version one accepts ordered image batches bounded by configurable per-message count and aggregate-byte limits plus per-image byte and pixel limits. The browser rejects unsupported declared formats before preview allocation, while the host authoritatively decodes the complete batch, checks current deployment bounds, validates every image without storage writes, and only then saves every image while preserving submitted order in the resulting durable blocks. Request buffering derives directly from the attachment service's aggregate image-byte limit.

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/simplification/2026-07-29-simplify-web-image-input-v1.md](../02-notes/implemented/simplification/2026-07-29-simplify-web-image-input-v1.md)
- Pinned source: [.agents/notes/implemented/simplification/2026-07-29-simplify-web-image-input-v1.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/simplification/2026-07-29-simplify-web-image-input-v1.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`packages/llm/llm/src/types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/src/types.ts) | public types and contract | Defines `ImageBlock`, a construct named by the note. | `symbol-definition` |
| [`packages/core/tools/src/py-types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/py-types.ts) | runtime implementation | Defines `describe`, a construct named by the note. | `symbol-definition` |
| [`packages/todo/tool-todo/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/todo/tool-todo/src/index.ts) | package entry point | Defines `describe`, a construct named by the note. | `symbol-definition` |
| [`packages/client/ui-conversation/src/client/service.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/src/client/service.ts) | runtime implementation | Defines `IConversation`, a construct named by the note. | `symbol-definition` |
| [`packages/client/ui-message-feedback/src/client/controller.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-message-feedback/src/client/controller.ts) | runtime implementation | Defines `describe`, a construct named by the note. | `symbol-definition` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `IConversation` | `interface` | [`packages/client/ui-conversation/src/client/service.ts:28`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/src/client/service.ts#L28) | `export interface IConversation {` |
| `describe` | `function` | [`packages/client/ui-message-feedback/src/client/controller.ts:80`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-message-feedback/src/client/controller.ts#L80) | `function describe(code: string): string {` |
| `describe` | `function` | [`packages/core/tools/src/py-types.ts:223`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/py-types.ts#L223) | `function describe(schema: object): string \| undefined {` |
| `ImageBlock` | `interface` | [`packages/llm/llm/src/types.ts:71`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/src/types.ts#L71) | `export interface ImageBlock {` |
| `describe` | `function` | [`packages/todo/tool-todo/src/index.ts:74`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/todo/tool-todo/src/index.ts#L74) | `function describe(allowParallel: boolean): string {` |

### Tests and executable evidence

- [`scripts/test-invariants.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/test-invariants.ts) — A test under the owning area exercises or imports `validateImage`. A test under the owning area exercises or imports `saveImage`.
- [`packages/llm/llm-pi-ai/tests/adapter.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm-pi-ai/tests/adapter.spec.ts) — A test under the owning area exercises or imports `validateImage`. A test under the owning area exercises or imports `saveImage`.
- [`packages/fs/tool-fs/tests/read-image.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/tool-fs/tests/read-image.spec.ts) — A test under the owning area exercises or imports `validateImage`. A test under the owning area exercises or imports `saveImage`.
- [`packages/llm/llm-pi-ai/tests/context.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm-pi-ai/tests/context.spec.ts) — A test under the owning area exercises or imports `readImage`.
- [`packages/llm/llm-pi-ai/tests/convert.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm-pi-ai/tests/convert.spec.ts) — A test under the owning area exercises or imports `readImage`.
- [`packages/llm/llm-pi-ai/tests/provider-apis.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm-pi-ai/tests/provider-apis.e2e.ts) — A test under the owning area exercises or imports `validateImage`. A test under the owning area exercises or imports `saveImage`.
- [`packages/host/apiproxy/tests/session-export.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/tests/session-export.spec.ts) — A test under the owning area exercises or imports `validateImage`. A test under the owning area exercises or imports `saveImage`.
- [`packages/host/apiproxy/tests/api-proxy-models.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/tests/api-proxy-models.spec.ts) — A test under the owning area exercises or imports `validateImage`. A test under the owning area exercises or imports `saveImage`.

## How to read the implementation

1. Start with [`packages/llm/llm/src/types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/src/types.ts) because it has the strongest evidence link to the note.
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

- Tags: `class/simplification`, `concern/boundary`, `concern/cancellation-timeout`, `concern/compatibility`, `concern/discovery-routing`, `concern/evidence`, `concern/lifecycle`, `concern/ownership`, `concern/schema-types`, `domain/build-release`, `domain/configuration`, `domain/extensions`, `domain/filesystem`, `domain/llm`, `domain/protocols`, `domain/session-state`, `domain/storage`, `domain/testing`, `domain/web-retrieval`, `lifecycle/implemented`, `mechanism/adapter`, `mechanism/event-sourcing`, `mechanism/policy`, `mechanism/registry`
- Aliases: `IConversation`, `describe`, `ImageBlock`, `host.describe`, `validateImage`, `saveImage`, `readImage`, `Simplify Web image input version one`, `simplification`, `boundary`, `cancellation timeout`, `compatibility`, `discovery routing`, `evidence`
- Regex: `(?i)(IConversation|describe|ImageBlock|host\.describe|validateImage|saveImage|readImage|Simplify[- ]Web[- ]image[- ]input[- ]version[- ]one)`

```bash
rg -n --pcre2 "(?i)(IConversation|describe|ImageBlock|host\\.describe|validateImage|saveImage|readImage|Simplify[- ]Web[- ]image[- ]input[- ]version[- ]one)" source-deepseek-harness
rg -l --fixed-strings "class/simplification" 03-agent-context
```

## Connected notes

- **`shares-code-with`** — [0511. Session projections and command lifecycle logging](0511-session-projections-and-command-lifecycle-logging.md): Shares source implementation: `packages/core/tools/src/py-types.ts`, `packages/todo/tool-todo/src/index.ts`.
- **`shares-code-with`** — [0500. Keyless browser e2e lane for the web GUI](0500-keyless-browser-e2e-lane-for-the-web-gui.md): Shares source implementation: `packages/llm/llm/src/types.ts`.
- **`shares-code-with`** — [0145. Explicit model-facing tool order](0145-explicit-model-facing-tool-order.md): Shares source implementation: `packages/core/tools/src/py-types.ts`, `packages/llm/llm/src/types.ts`.
- **`shares-code-with`** — [0190. Allow several `in_progress` todos at once](0190-allow-several-in-progress-todos-at-once.md): Shares source implementation: `packages/client/ui-conversation/src/client/service.ts`, `packages/todo/tool-todo/src/index.ts`.
- **`shares-code-with`** — [0078. Terminal LLM stream failures](0078-terminal-llm-stream-failures.md): Shares source implementation: `packages/core/tools/src/py-types.ts`, `packages/llm/llm/src/types.ts`.
- **`shares-code-with`** — [0516. Migrate simple unary API Proxy calls to business Remote services](0516-migrate-simple-unary-api-proxy-calls-to-business-remote-services.md): Shares source implementation: `packages/llm/llm/src/types.ts`.
- **`shares-code-with`** — [0147. MCP client plugin --- connect to external MCP servers and bridge their tools](0147-mcp-client-plugin-connect-to-external-mcp-servers-and-bridge-their-tools.md): Shares source implementation: `packages/core/tools/src/py-types.ts`.
- **`shares-code-with`** — [0432. Unified GitHub label taxonomy](0432-unified-github-label-taxonomy.md): Shares source implementation: `packages/core/tools/src/py-types.ts`, `packages/llm/llm/src/types.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0468-simplify-web-image-input-version-one.md`.
