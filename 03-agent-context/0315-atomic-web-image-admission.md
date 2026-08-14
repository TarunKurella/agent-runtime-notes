---
id: "dsh-note-0315"
title: "Atomic Web image admission"
status: "implemented"
class: "bug-fix"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/bug-fix/2026-07-29-atomic-web-image-admission.md"
implementation_evidence: "lead-only"
target_anchor: "Rust provider-adapter boundary"
tags:
  - "class/bug-fix"
  - "concern/boundary"
  - "concern/concurrency"
  - "concern/discovery-routing"
  - "concern/evidence"
  - "concern/human-control"
  - "concern/ownership"
  - "concern/performance"
  - "domain/context"
  - "domain/llm"
  - "domain/session-state"
  - "domain/storage"
  - "domain/testing"
  - "lifecycle/implemented"
aliases:
  - "session.selectModel"
  - "selectModel"
  - "user/message"
  - "steering/message"
  - "Session.deriveMessages"
  - "Atomic Web image admission"
  - "bug fix"
  - "boundary"
  - "concurrency"
  - "discovery routing"
  - "evidence"
  - "human control"
  - "ownership"
  - "performance"
search_regex: "(?i)(session\\.selectModel|selectModel|user/message|steering/message|Session\\.deriveMessages|Atomic[- ]Web[- ]image[- ]admission|bug[- ]fix|boundary)"
---

# 0315. Atomic Web image admission — implementation context

## Open this when

Image prompt admission and session.selectModel each read session modality state across asynchronous model and attachment lookups. Without one ordering boundary, an image prompt could validate an image-capable target while a concurrent selection installed a text-only target, or selection could miss a prompt after inbox dequeue but before its durable message event. Scanning the immutable event log avoided the second race but permanently blocked a text-only selection even after compaction removed the image from current model history.

## Source decision

Each live Web agent has one private promise chain shared by image-bearing prompt admission and model selection. A failed operation settles its caller normally and leaves the chain usable. Text-only prompts bypass the chain because they cannot change the modality constraint. The pending-publication set records a queued occurrence at dequeue and a steering occurrence already at enqueue (steering items never enter the queued UI mirror), and retains each until its matching user/message or steering/message event publishes.

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/bug-fix/2026-07-29-atomic-web-image-admission.md](../02-notes/implemented/bug-fix/2026-07-29-atomic-web-image-admission.md)
- Pinned source: [.agents/notes/implemented/bug-fix/2026-07-29-atomic-web-image-admission.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/bug-fix/2026-07-29-atomic-web-image-admission.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`docs/architecture.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/architecture.md) | package contract and examples | Contains the exact code literal `user/message` named by the note. | `exact-code-occurrence` |
| [`docs/tool-catalog.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/tool-catalog.md) | package contract and examples | Contains the exact code literal `user/message` named by the note. | `exact-code-occurrence` |
| [`docs/tool-catalog.zh.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/tool-catalog.zh.md) | package contract and examples | Contains the exact code literal `user/message` named by the note. | `exact-code-occurrence` |
| [`docs/architecture.zh.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/architecture.zh.md) | package contract and examples | Contains the exact code literal `user/message` named by the note. | `exact-code-occurrence` |
| [`docs/agent-lifecycle.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/agent-lifecycle.md) | package contract and examples | Contains the exact code literal `user/message` named by the note. | `exact-code-occurrence` |
| [`docs/subsystems/core.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/subsystems/core.md) | package contract and examples | Contains the exact code literal `steering/message` named by the note. | `exact-code-occurrence` |
| [`scripts/gen-doc-graphs.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/gen-doc-graphs.ts) | repository automation | Contains the exact code literal `user/message` named by the note. | `exact-code-occurrence` |
| [`docs/subsystems/core.zh.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/subsystems/core.zh.md) | package contract and examples | Contains the exact code literal `steering/message` named by the note. | `exact-code-occurrence` |
| [`scripts/gen-tool-catalog.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/gen-tool-catalog.ts) | repository automation | Contains the exact code literal `user/message` named by the note. | `exact-code-occurrence` |
| [`docs/persistence-catalog.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/persistence-catalog.md) | package contract and examples | Contains the exact code literal `user/message` named by the note. | `exact-code-occurrence` |
| [`packages/session/session-persistence/README.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/session/session-persistence/README.md) | package contract and examples | Contains the exact code literal `steering/message` named by the note. | `exact-code-occurrence` |
| [`packages/session/session-persistence/README.zh.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/session/session-persistence/README.zh.md) | package contract and examples | Contains the exact code literal `steering/message` named by the note. | `exact-code-occurrence` |

### Code constructs named by the note

The note names behavior or policy but no uniquely resolvable code declaration. Search by its aliases and inspect the listed package entry points.

### Tests and executable evidence

- [`packages/client/runtime/tests/fake-api.client.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/runtime/tests/fake-api.client.ts) — A test under the owning area exercises or imports `selectModel`.
- [`packages/host/apiproxy/tests/fetch-carrier.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/tests/fetch-carrier.spec.ts) — A test under the owning area exercises or imports `selectModel`.
- [`packages/client/connection/tests/fake-api.client.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/connection/tests/fake-api.client.ts) — A test under the owning area exercises or imports `selectModel`.
- [`packages/host/apiproxy/tests/client-handler.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/tests/client-handler.spec.ts) — A test under the owning area exercises or imports `selectModel`.
- [`packages/host/apiproxy/tests/api-proxy-models.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/tests/api-proxy-models.spec.ts) — A test under the owning area exercises or imports `selectModel`.
- [`packages/client/connection/tests/fixture.client.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/connection/tests/fixture.client.spec.ts) — A test under the owning area exercises or imports `selectModel`.
- [`packages/client/ui-model-selection/tests/browser-plugin.client.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-model-selection/tests/browser-plugin.client.spec.ts) — A test under the owning area exercises or imports `selectModel`.
- [`packages/core/agent-loop/tests/resume.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent-loop/tests/resume.spec.ts) — Contains the exact code literal `steering/message` named by the note.

## How to read the implementation

1. Start with [`docs/architecture.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/architecture.md) because it has the strongest evidence link to the note.
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
- **`rust/trait-seam`** — Use a small trait at the capability boundary; keep policy and model-facing schemas outside the backend implementation.

### Smallest safe implementation slice

1. Express the semantic contract as Rust types and one owning boundary.
2. Add the smallest adapter or service needed by the existing loop; do not add a new model-facing tool unless the four-tool surface cannot express the job.
3. Record durable evidence before deriving summaries or UI state.
4. Add one happy-path test, one interruption test, and one replay/restart test where state survives a turn.
5. Keep the patch reversible until the source note's strongest failure case is proven in the Rust harness.

## Search handles

- Tags: `class/bug-fix`, `concern/boundary`, `concern/concurrency`, `concern/discovery-routing`, `concern/evidence`, `concern/human-control`, `concern/ownership`, `concern/performance`, `domain/context`, `domain/llm`, `domain/session-state`, `domain/storage`, `domain/testing`, `lifecycle/implemented`
- Aliases: `session.selectModel`, `selectModel`, `user/message`, `steering/message`, `Session.deriveMessages`, `Atomic Web image admission`, `bug fix`, `boundary`, `concurrency`, `discovery routing`, `evidence`, `human control`, `ownership`, `performance`
- Regex: `(?i)(session\.selectModel|selectModel|user/message|steering/message|Session\.deriveMessages|Atomic[- ]Web[- ]image[- ]admission|bug[- ]fix|boundary)`

```bash
rg -n --pcre2 "(?i)(session\\.selectModel|selectModel|user/message|steering/message|Session\\.deriveMessages|Atomic[- ]Web[- ]image[- ]admission|bug[- ]fix|boundary)" source-deepseek-harness
rg -l --fixed-strings "class/bug-fix" 03-agent-context
```

## Connected notes

- **`shares-code-with`** — [0191. Native workspace directory picker](0191-native-workspace-directory-picker.md): Shares source implementation: `packages/client/connection/tests/fake-api.client.ts`, `packages/client/runtime/tests/fake-api.client.ts`.
- **`shares-code-with`** — [0537. Truncate interrupted final turns on load](0537-truncate-interrupted-final-turns-on-load.md): Shares source implementation: `docs/architecture.md`, `docs/tool-catalog.md`.
- **`shares-code-with`** — [0621. TUI step timing trails the step's last message](0621-tui-step-timing-trails-the-step-s-last-message.md): Shares source implementation: `docs/architecture.md`, `docs/tool-catalog.md`.
- **`shares-code-with`** — [0481. Conversational Schedule delivery](0481-conversational-schedule-delivery.md): Shares source implementation: `docs/tool-catalog.md`, `docs/tool-catalog.zh.md`.
- **`shares-code-with`** — [0131. Code Mode --- the model writes TypeScript against the tool registry](0131-code-mode-the-model-writes-typescript-against-the-tool-registry.md): Shares source implementation: `docs/architecture.md`.
- **`shares-code-with`** — [0206. Tool-call file open in OS](0206-tool-call-file-open-in-os.md): Shares source implementation: `packages/host/apiproxy/tests/client-handler.spec.ts`, `packages/host/apiproxy/tests/fetch-carrier.spec.ts`.
- **`shares-code-with`** — [0582. The running status line shows the turn phase and elapsed time](0582-the-running-status-line-shows-the-turn-phase-and-elapsed-time.md): Shares source implementation: `docs/architecture.md`, `docs/tool-catalog.md`.
- **`shares-code-with`** — [0392. Package Model Experience contract](0392-package-model-experience-contract.md): Shares source implementation: `docs/tool-catalog.md`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0315-atomic-web-image-admission.md`.
