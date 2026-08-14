---
id: "dsh-note-0457"
title: "Project injected content verbatim, dropping the XML envelopes"
status: "implemented"
class: "simplification"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/simplification/2026-07-20-unwrap-injected-content-envelopes.md"
implementation_evidence: "medium"
target_anchor: "Rust provider-adapter boundary"
tags:
  - "class/simplification"
  - "concern/boundary"
  - "concern/human-control"
  - "concern/lifecycle"
  - "concern/ownership"
  - "domain/context"
  - "domain/extensions"
  - "domain/llm"
  - "domain/protocols"
  - "domain/session-state"
  - "domain/storage"
  - "domain/ui-interaction"
  - "lifecycle/implemented"
  - "mechanism/adapter"
  - "mechanism/projection"
aliases:
  - "inject"
  - "source"
  - "meta"
  - "envelope"
  - "deriveEventMessage"
  - "additionalContexts"
  - "SessionEventMap"
  - "raw"
  - "steering/message"
  - "<steering source=\"…\">…</steering>"
  - "context/message"
  - "<context source=\"…\">…</context>"
  - "agent-instructions"
  - "<system-reminder>"
search_regex: "(?i)(inject|source|meta|envelope|deriveEventMessage|additionalContexts|SessionEventMap|steering/message)"
---

# 0457. Project injected content verbatim, dropping the XML envelopes — implementation context

## Open this when

Two families of injected session content rendered into the model transcript wrapped in XML envelopes: steering/message as … and context/message as … (the latter with a 'raw' opt-out that skipped the wrapper). The envelopes aimed to tell the model "this is injected, not the user speaking." Two problems: No model is trained on these tags. and are arbitrary markup no model was taught to read, so the framing adds tokens without a reliable effect and can actively mislead --- recorded transcripts show a model treating a instruction as third-party metadata and refusing it while answering only the original prompt.

## Source decision

Injected session content projects verbatim; the caller owns any framing. deriveEventMessage renders user/message content blocks to the model unchanged; source stays in the durable event log but does not render. The ContextEnvelope type and every envelope field are removed --- context/message in SessionEventMap, InjectOptions, HookContext, and the inject()/additionalContexts plumbing in dsh-agent-loop. agent-instructions no longer requests 'raw'; its self-framed content renders as before. The renderTagged/renderContextEnvelope helpers are deleted.

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/simplification/2026-07-20-unwrap-injected-content-envelopes.md](../02-notes/implemented/simplification/2026-07-20-unwrap-injected-content-envelopes.md)
- Pinned source: [.agents/notes/implemented/simplification/2026-07-20-unwrap-injected-content-envelopes.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/simplification/2026-07-20-unwrap-injected-content-envelopes.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`packages/core/agent-loop/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent-loop/src/index.ts) | package entry point | Core file in the package named by the note: `packages/core/agent-loop`. Defines `source`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/core/agent-loop/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent-loop/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/core/agent-loop`. Defines `inject`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/context/agent-instructions/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/context/agent-instructions/src/index.ts) | package entry point | Core file in the package named by the note: `packages/context/agent-instructions`. | `named-package-member` |
| [`packages/context/agent-instructions/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/context/agent-instructions/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/context/agent-instructions`. Defines `inject`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/core/agent-loop`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent-loop) | package or module directory | The note names this package or capability. | `named-package` |
| [`packages/context/agent-instructions`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/packages/context/agent-instructions) | package or module directory | The note names this package or capability. | `named-package` |
| [`vendor/loader/src/internal.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/loader/src/internal.ts) | runtime implementation | Defines `raw`, a construct named by the note. | `symbol-definition` |
| [`packages/core/tools/src/types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/types.ts) | public types and contract | Defines `SessionEventMap`, a construct named by the note. | `symbol-definition` |
| [`packages/core/tools/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/index.ts) | package entry point | Defines `additionalContexts`, a construct named by the note. | `symbol-definition` |
| [`packages/core/session/src/surface.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/surface.ts) | runtime implementation | Defines `deriveEventMessage`, a construct named by the note. | `symbol-definition` |
| [`packages/core/session/src/chunk-rows.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/chunk-rows.ts) | runtime implementation | Defines `envelope`, a construct named by the note. | `symbol-definition` |
| [`packages/core/agent-loop/README.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent-loop/README.md) | package contract and examples | Core file in the package named by the note: `packages/core/agent-loop`. | `named-package-member` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `inject` | `const` | [`packages/context/agent-instructions/src/invariant.ts:15`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/context/agent-instructions/src/invariant.ts#L15) | `export const inject = ['invariants']` |
| `source` | `let` | [`packages/core/agent-loop/src/index.ts:324`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent-loop/src/index.ts#L324) | `let source: () => AgentLoopSettings = () => entry` |
| `meta` | `const` | [`packages/core/agent-loop/src/index.ts:356`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent-loop/src/index.ts#L356) | `const meta = cwd === undefined ? {} : { cwd }` |
| `inject` | `const` | [`packages/core/agent-loop/src/invariant.ts:16`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent-loop/src/invariant.ts#L16) | `export const inject = ['invariants']` |
| `envelope` | `const` | [`packages/core/session/src/chunk-rows.ts:162`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/chunk-rows.ts#L162) | `const envelope = { seq0: first.seq, time0: first.time }` |
| `deriveEventMessage` | `function` | [`packages/core/session/src/surface.ts:83`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/surface.ts#L83) | `export function deriveEventMessage(event: SessionEvent): Message \| null {` |
| `additionalContexts` | `const` | [`packages/core/tools/src/index.ts:1760`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/index.ts#L1760) | `const additionalContexts = [` |
| `SessionEventMap` | `interface` | [`packages/core/tools/src/types.ts:26`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/types.ts#L26) | `interface SessionEventMap {` |
| `raw` | `const` | [`vendor/loader/src/internal.ts:125`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/loader/src/internal.ts#L125) | `const raw = requireInternal('internal/modules/esm/loader')?.getOrInitializeCascadedLoader()` |

### Tests and executable evidence

- [`packages/core/tools/tests/tools.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/tests/tools.spec.ts) — A test under the owning area exercises or imports `additionalContexts`.
- [`packages/core/agent-loop/tests/loop.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent-loop/tests/loop.spec.ts) — A test under the owning area exercises or imports `agent-instructions`. A test under the owning area exercises or imports `dsh-agent-loop`.
- [`packages/core/session/tests/session.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/tests/session.spec.ts) — A test under the owning area exercises or imports `envelope`.
- [`packages/core/tools/tests/code-mode.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/tests/code-mode.spec.ts) — A test under the owning area exercises or imports `SessionEventMap`. A test under the owning area exercises or imports `additionalContexts`.
- [`packages/core/session/tests/chunk-rows.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/tests/chunk-rows.spec.ts) — A test under the owning area exercises or imports `envelope`.
- [`packages/core/agent-loop/tests/settings.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent-loop/tests/settings.spec.ts) — A test under the owning area exercises or imports `dsh-agent-loop`.
- [`packages/core/session/tests/derived-cache.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/tests/derived-cache.spec.ts) — A test under the owning area exercises or imports `deriveEventMessage`.
- [`packages/core/agent-loop/tests/tool-calls.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent-loop/tests/tool-calls.spec.ts) — A test under the owning area exercises or imports `additionalContexts`. A test under the owning area exercises or imports `dsh-agent-loop`.

## How to read the implementation

1. Start with [`packages/core/agent-loop/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent-loop/src/index.ts) because it has the strongest evidence link to the note.
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

- Tags: `class/simplification`, `concern/boundary`, `concern/human-control`, `concern/lifecycle`, `concern/ownership`, `domain/context`, `domain/extensions`, `domain/llm`, `domain/protocols`, `domain/session-state`, `domain/storage`, `domain/ui-interaction`, `lifecycle/implemented`, `mechanism/adapter`, `mechanism/projection`
- Aliases: `inject`, `source`, `meta`, `envelope`, `deriveEventMessage`, `additionalContexts`, `SessionEventMap`, `raw`, `steering/message`, `<steering source="…">…</steering>`, `context/message`, `<context source="…">…</context>`, `agent-instructions`, `<system-reminder>`
- Regex: `(?i)(inject|source|meta|envelope|deriveEventMessage|additionalContexts|SessionEventMap|steering/message)`

```bash
rg -n --pcre2 "(?i)(inject|source|meta|envelope|deriveEventMessage|additionalContexts|SessionEventMap|steering/message)" source-deepseek-harness
rg -l --fixed-strings "class/simplification" 03-agent-context
```

## Connected notes

- **`source-link`** — [0001. Provider-neutral content-block vocabulary owned by dsh-llm](0001-provider-neutral-content-block-vocabulary-owned-by-dsh-llm.md): The source note links to this decision directly.
- **`shares-code-with`** — [0305. Semantic session checkpoints](0305-semantic-session-checkpoints.md): Shares source implementation: `packages/core/agent-loop`, `packages/core/agent-loop/src/index.ts`.
- **`shares-code-with`** — [0259. Per-agent tool presentation, and the `code` preset](0259-per-agent-tool-presentation-and-the-code-preset.md): Shares source implementation: `packages/core/agent-loop/src/index.ts`, `packages/core/agent-loop/src/invariant.ts`.
- **`shares-code-with`** — [0163. Plugin-owned human command registration](0163-plugin-owned-human-command-registration.md): Shares source implementation: `packages/core/agent-loop`, `packages/core/agent-loop/src/index.ts`.
- **`shares-code-with`** — [0221. Read card --- the read tool's structured line window reaches the client](0221-read-card-the-read-tool-s-structured-line-window-reaches-the-client.md): Shares source implementation: `packages/context/agent-instructions`, `packages/context/agent-instructions/src/index.ts`.
- **`shares-code-with`** — [0159. Fresh-agent Ralph workflow tool](0159-fresh-agent-ralph-workflow-tool.md): Shares source implementation: `packages/core/agent-loop/src/index.ts`, `packages/core/tools/src/index.ts`.
- **`shares-code-with`** — [0136. Workspace context instruction files](0136-workspace-context-instruction-files.md): Shares source implementation: `packages/context/agent-instructions`, `packages/context/agent-instructions/src/index.ts`.
- **`shares-code-with`** — [0004. Microkernel --- extension via Cordis event taxonomy, one concrete loop](0004-microkernel-extension-via-cordis-event-taxonomy-one-concrete-loop.md): Shares source implementation: `packages/core/agent-loop`, `packages/core/agent-loop/src/index.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0457-project-injected-content-verbatim-dropping-the-xml-envelopes.md`.
