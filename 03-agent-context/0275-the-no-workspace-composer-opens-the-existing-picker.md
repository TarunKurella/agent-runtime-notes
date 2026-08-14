---
id: "dsh-note-0275"
title: "The no-Workspace composer opens the existing picker"
status: "implemented"
class: "feature"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/feature/2026-08-07-workspace-picker-composer-entry.md"
implementation_evidence: "medium"
target_anchor: "sandbox capability boundary"
tags:
  - "class/feature"
  - "concern/boundary"
  - "concern/concurrency"
  - "concern/discovery-routing"
  - "concern/evidence"
  - "concern/lifecycle"
  - "concern/ownership"
  - "concern/recovery"
  - "concern/trust"
  - "domain/filesystem"
  - "domain/llm"
  - "domain/security"
  - "domain/session-state"
  - "domain/ui-interaction"
  - "domain/web-retrieval"
  - "lifecycle/implemented"
aliases:
  - "ConversationRoot"
  - "WorkspacePicker"
  - "workspace"
  - "conversation.hero.workspace"
  - "aria-haspopup=\"menu"
  - "aria-expanded"
  - "The no-Workspace composer opens the existing picker"
  - "feature"
  - "boundary"
  - "concurrency"
  - "discovery routing"
  - "evidence"
  - "lifecycle"
  - "ownership"
search_regex: "(?i)(ConversationRoot|WorkspacePicker|workspace|conversation\\.hero\\.workspace|aria\\-haspopup=\"menu|aria\\-expanded|The[- ]no\\-Workspace[- ]composer[- ]opens[- ]the[- ]existing[- ]picker|feature)"
---

# 0275. The no-Workspace composer opens the existing picker — implementation context

## Open this when

The session-scope decision keeps one resident composer before a Workspace exists, but its textarea was disabled and only the smaller Workspace chip could open the picker. The largest and most familiar starting affordance therefore rejected the user's first click even though a recovery action was available on the same surface.

## Source decision

While no Workspace owns the new Session, the whole composer card activates the existing conversation.hero.workspace picker by pointer click --- the card owns the click handler and its disabled controls let pointer events fall through, so the full capsule is one target --- and the read-only resident textarea does the same by Enter or Space. aria-haspopup="menu" and aria-expanded describe the shared picker menu while it is mounted.

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/feature/2026-08-07-workspace-picker-composer-entry.md](../02-notes/implemented/feature/2026-08-07-workspace-picker-composer-entry.md)
- Pinned source: [.agents/notes/implemented/feature/2026-08-07-workspace-picker-composer-entry.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/feature/2026-08-07-workspace-picker-composer-entry.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`packages/host/apiproxy/src/api-proxy.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/src/api-proxy.ts) | runtime implementation | Defines `workspace`, a construct named by the note. | `symbol-definition` |
| [`packages/client/ui-workspace/src/client/WorkspacePicker.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-workspace/src/client/WorkspacePicker.tsx) | runtime implementation | Defines `WorkspacePicker`, a construct named by the note. | `symbol-definition` |
| [`packages/client/ui-conversation/src/client/skeleton/ConversationRoot.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/src/client/skeleton/ConversationRoot.tsx) | runtime implementation | Defines `ConversationRoot`, a construct named by the note. | `symbol-definition` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `ConversationRoot` | `function` | [`packages/client/ui-conversation/src/client/skeleton/ConversationRoot.tsx:15`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/src/client/skeleton/ConversationRoot.tsx#L15) | `export function ConversationRoot({` |
| `WorkspacePicker` | `function` | [`packages/client/ui-workspace/src/client/WorkspacePicker.tsx:225`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-workspace/src/client/WorkspacePicker.tsx#L225) | `export function WorkspacePicker({` |
| `workspace` | `const` | [`packages/host/apiproxy/src/api-proxy.ts:1519`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/src/api-proxy.ts#L1519) | `const workspace = workspaces.find(candidate => candidate.sessionIds.includes(ancestor.header.id))` |

### Tests and executable evidence

- [`packages/typert/generator/tests/fixtures/type-model`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/typert/generator/tests/fixtures/type-model) — The note names this package or capability.
- [`packages/typert/generator/tests/fixtures/type-model/package.json`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/typert/generator/tests/fixtures/type-model/package.json) — Core file in the package named by the note: `packages/typert/generator/tests/fixtures/type-model`.
- [`packages/client/ui-workspace/tests/apply.client.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-workspace/tests/apply.client.spec.ts) — A test under the owning area exercises or imports `WorkspacePicker`.
- [`packages/client/ui-conversation/tests/skeleton.client.spec.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/tests/skeleton.client.spec.tsx) — A test under the owning area exercises or imports `ConversationRoot`.
- [`packages/client/ui-workspace/tests/workspace-picker.client.spec.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-workspace/tests/workspace-picker.client.spec.tsx) — A test under the owning area exercises or imports `WorkspacePicker`.
- [`packages/client/ui-conversation/tests/views-type-chain.client.spec.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/tests/views-type-chain.client.spec.tsx) — A test under the owning area exercises or imports `ConversationRoot`.

## How to read the implementation

1. Start with [`packages/host/apiproxy/src/api-proxy.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/src/api-proxy.ts) because it has the strongest evidence link to the note.
2. Read public types and entry points before implementation details. Write down the contract in plain language.
3. Follow the named constructs into their callers and tests. Confirm which component can actually enforce the rule.
4. Reconstruct one failure path: cancellation, crash, retry, stale state, partial write, or incompatible provider input—whichever the note is protecting against.
5. Only then map the contract into the current Rust repository. Target paths must come from that repository, never from guesswork in this wiki.

## Rust harness port

**Likely integration point:** sandbox capability boundary.

Preserve the behavior and ownership rule. Do not mechanically translate Cordis, TypeScript declaration merging, Node streams, or package topology into Rust.

Relevant standing rules:

- **`rust/append-log`** — Keep durable facts append-only and derive views from them. Separate the synchronous fact boundary from asynchronous persistence.
- **`rust/capability-security`** — Make granted capabilities explicit. On the Windows no-admin path, probe support at runtime and fail closed rather than silently running unsandboxed.
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

- Tags: `class/feature`, `concern/boundary`, `concern/concurrency`, `concern/discovery-routing`, `concern/evidence`, `concern/lifecycle`, `concern/ownership`, `concern/recovery`, `concern/trust`, `domain/filesystem`, `domain/llm`, `domain/security`, `domain/session-state`, `domain/ui-interaction`, `domain/web-retrieval`, `lifecycle/implemented`
- Aliases: `ConversationRoot`, `WorkspacePicker`, `workspace`, `conversation.hero.workspace`, `aria-haspopup="menu`, `aria-expanded`, `The no-Workspace composer opens the existing picker`, `feature`, `boundary`, `concurrency`, `discovery routing`, `evidence`, `lifecycle`, `ownership`
- Regex: `(?i)(ConversationRoot|WorkspacePicker|workspace|conversation\.hero\.workspace|aria\-haspopup="menu|aria\-expanded|The[- ]no\-Workspace[- ]composer[- ]opens[- ]the[- ]existing[- ]picker|feature)`

```bash
rg -n --pcre2 "(?i)(ConversationRoot|WorkspacePicker|workspace|conversation\\.hero\\.workspace|aria\\-haspopup=\"menu|aria\\-expanded|The[- ]no\\-Workspace[- ]composer[- ]opens[- ]the[- ]existing[- ]picker|feature)" source-deepseek-harness
rg -l --fixed-strings "class/feature" 03-agent-context
```

## Connected notes

- **`source-link`** — [0061. Web client Agent-scope parity model and the provisioning channel (agents/scope / blank reuse / provide)](0061-web-client-agent-scope-parity-model-and-the-provisioning-channel-agents.md): The source note links to this decision directly.
- **`shares-code-with`** — [0239. Goal command input projection](0239-goal-command-input-projection.md): Shares source implementation: `packages/client/ui-conversation/src/client/skeleton/ConversationRoot.tsx`, `packages/client/ui-conversation/tests/skeleton.client.spec.tsx`.
- **`shares-code-with`** — [0472. One route to add a Workspace](0472-one-route-to-add-a-workspace.md): Shares source implementation: `packages/client/ui-workspace/src/client/WorkspacePicker.tsx`, `packages/host/apiproxy/src/api-proxy.ts`.
- **`shares-code-with`** — [0318. Fixed header, sticky composer inside the transcript scrollport](0318-fixed-header-sticky-composer-inside-the-transcript-scrollport.md): Shares source implementation: `packages/client/ui-conversation/src/client/skeleton/ConversationRoot.tsx`, `packages/client/ui-conversation/tests/skeleton.client.spec.tsx`.
- **`shares-code-with`** — [0185. Workspace UI Complete Product Flow](0185-workspace-ui-complete-product-flow.md): Shares source implementation: `packages/host/apiproxy/src/api-proxy.ts`, `packages/typert/generator/tests/fixtures/type-model`.
- **`shares-code-with`** — [0183. Session List Browsing and Manual Workspace Order](0183-session-list-browsing-and-manual-workspace-order.md): Shares source implementation: `packages/host/apiproxy/src/api-proxy.ts`, `packages/typert/generator/tests/fixtures/type-model`.
- **`shares-code-with`** — [0194. Trajectory inspection ledger](0194-trajectory-inspection-ledger.md): Shares source implementation: `packages/client/ui-conversation/src/client/skeleton/ConversationRoot.tsx`, `packages/host/apiproxy/src/api-proxy.ts`.
- **`shares-code-with`** — [0625. Hero stays visible while a blank session opens](0625-hero-stays-visible-while-a-blank-session-opens.md): Shares source implementation: `packages/client/ui-conversation/src/client/skeleton/ConversationRoot.tsx`, `packages/host/apiproxy/src/api-proxy.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0275-the-no-workspace-composer-opens-the-existing-picker.md`.
