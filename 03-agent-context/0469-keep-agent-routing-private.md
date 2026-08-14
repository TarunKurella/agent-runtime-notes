---
id: "dsh-note-0469"
title: "Keep agent routing private"
status: "implemented"
class: "simplification"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/simplification/2026-07-30-private-agent-send.md"
implementation_evidence: "medium"
target_anchor: "Rust provider-adapter boundary"
tags:
  - "class/simplification"
  - "concern/boundary"
  - "concern/cancellation-timeout"
  - "concern/concurrency"
  - "concern/discovery-routing"
  - "concern/evidence"
  - "concern/human-control"
  - "concern/lifecycle"
  - "domain/build-release"
  - "domain/extensions"
  - "domain/llm"
  - "domain/testing"
  - "lifecycle/implemented"
  - "mechanism/policy"
aliases:
  - "cancel"
  - "ReactLoopAgent"
  - "inject"
  - "send"
  - "Agent.send"
  - "next-turn"
  - "SendTarget"
  - "SendOptions"
  - "dsh-agent"
  - "user/message"
  - "Keep agent routing private"
  - "simplification"
  - "boundary"
  - "cancellation timeout"
search_regex: "(?i)(cancel|ReactLoopAgent|inject|send|Agent\\.send|next\\-turn|SendTarget|SendOptions)"
---

# 0469. Keep agent routing private — implementation context

## Open this when

The public Agent.send() method exposed the concrete loop's routing matrix even though production callers use only the semantic followup(), steer(), and inject() operations. Its fourth combination, next-turn with wakeup: false, had no consumer beyond tests. Keeping that latent capability public also required alternate Agent implementations and test fakes to accept implementation-level routing policy.

## Source decision

Agent exposes followup(), steer(), and inject() as its complete delivery contract. ReactLoopAgent keeps a private send() helper that shares routing mechanics among those methods, while SendTarget and SendOptions are no longer exported from dsh-agent. The public interface cannot queue a turn without waking the driver. A follow-up always requests execution, steering requests the nearest step, and injection supplies model-facing context without requesting execution.

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/simplification/2026-07-30-private-agent-send.md](../02-notes/implemented/simplification/2026-07-30-private-agent-send.md)
- Pinned source: [.agents/notes/implemented/simplification/2026-07-30-private-agent-send.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/simplification/2026-07-30-private-agent-send.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`packages/core/agent/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent/src/index.ts) | package entry point | Core file in the package named by the note: `packages/core/agent`. | `named-package-member` |
| [`packages/core/agent/src/types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent/src/types.ts) | public types and contract | Core file in the package named by the note: `packages/core/agent`. | `named-package-member` |
| [`packages/core/agent/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/core/agent`. Defines `inject`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/core/agent/src/runtime-types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent/src/runtime-types.ts) | runtime implementation | Core file in the package named by the note: `packages/core/agent`. | `named-package-member` |
| [`packages/core/agent`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent) | package or module directory | The note names this package or capability. | `named-package` |
| [`packages/core/agent-loop/src/agent.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent-loop/src/agent.ts) | runtime implementation | Defines `ReactLoopAgent`, a construct named by the note. | `symbol-definition` |
| [`packages/lsp/lsp-stdio/src/instance.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/lsp/lsp-stdio/src/instance.ts) | runtime implementation | Defines `send`, a construct named by the note. | `symbol-definition` |
| [`packages/client/ui-primitives/src/pointer-grace.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-primitives/src/pointer-grace.ts) | runtime implementation | Defines `cancel`, a construct named by the note. | `symbol-definition` |
| [`packages/core/agent/README.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent/README.md) | package contract and examples | Core file in the package named by the note: `packages/core/agent`. | `named-package-member` |
| [`packages/core/agent/package.json`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent/package.json) | composition and configuration | Core file in the package named by the note: `packages/core/agent`. | `named-package-member` |
| [`pnpm-lock.yaml`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/pnpm-lock.yaml) | composition and configuration | Contains the exact code literal `dsh-agent` named by the note. | `exact-code-occurrence` |
| [`tsconfig.base.json`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/tsconfig.base.json) | composition and configuration | Contains the exact code literal `dsh-agent` named by the note. | `exact-code-occurrence` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `cancel` | `const` | [`packages/client/ui-primitives/src/pointer-grace.ts:36`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-primitives/src/pointer-grace.ts#L36) | `const cancel = useCallback(() => {` |
| `ReactLoopAgent` | `class` | [`packages/core/agent-loop/src/agent.ts:64`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent-loop/src/agent.ts#L64) | `export class ReactLoopAgent implements Agent {` |
| `inject` | `const` | [`packages/core/agent/src/invariant.ts:12`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent/src/invariant.ts#L12) | `export const inject = ['invariants']` |
| `send` | `const` | [`packages/lsp/lsp-stdio/src/instance.ts:206`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/lsp/lsp-stdio/src/instance.ts#L206) | `const send = this.connection.request(requestMethod(operation), params)` |

### Tests and executable evidence

- [`packages/core/agent/tests/agent.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent/tests/agent.spec.ts) — A test under the owning area exercises or imports `steer`. A test under the owning area exercises or imports `next-turn`.
- [`packages/lsp/lsp-stdio/tests/fixture-server.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/lsp/lsp-stdio/tests/fixture-server.ts) — A test under the owning area exercises or imports `send`.
- [`packages/core/agent/tests/consumed-work.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent/tests/consumed-work.spec.ts) — A test under the owning area exercises or imports `next-turn`.
- [`packages/core/agent-loop/tests/contract-regressions.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent-loop/tests/contract-regressions.spec.ts) — A test under the owning area exercises or imports `ReactLoopAgent`.
- [`scripts/client-bundle-purity.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/client-bundle-purity.spec.ts) — Contains the exact code literal `dsh-agent` named by the note.
- [`scripts/verify-dsh-package-licenses.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/verify-dsh-package-licenses.spec.ts) — Contains the exact code literal `dsh-agent` named by the note.

## How to read the implementation

1. Start with [`packages/core/agent/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent/src/index.ts) because it has the strongest evidence link to the note.
2. Read public types and entry points before implementation details. Write down the contract in plain language.
3. Follow the named constructs into their callers and tests. Confirm which component can actually enforce the rule.
4. Reconstruct one failure path: cancellation, crash, retry, stale state, partial write, or incompatible provider input—whichever the note is protecting against.
5. Only then map the contract into the current Rust repository. Target paths must come from that repository, never from guesswork in this wiki.

## Rust harness port

**Likely integration point:** Rust provider-adapter boundary.

Preserve the behavior and ownership rule. Do not mechanically translate Cordis, TypeScript declaration merging, Node streams, or package topology into Rust.

Relevant standing rules:

- **`rust/four-tool-surface`** — Preserve the four model tools: read, write, patch, and exec. Reuse internal capabilities without automatically exposing new model-facing tools.
- **`rust/process-discipline`** — Treat this as repository or release policy unless it changes a runtime contract. Do not add runtime machinery for a process-only lesson.
- **`rust/provider-adapter`** — Keep provider wire quirks inside adapters; the loop and durable event vocabulary remain provider-neutral.
- **`rust/quickjs-boundary`** — QuickJS holds orchestration variables; Rust/Tokio owns async execution and exposes only explicitly granted capabilities.
- **`rust/tokio-cancellation`** — Thread one cancellation token through async work, close owned resources before reuse, and test cancellation at every await boundary.
- **`rust/trait-seam`** — Use a small trait at the capability boundary; keep policy and model-facing schemas outside the backend implementation.

### Smallest safe implementation slice

1. Express the semantic contract as Rust types and one owning boundary.
2. Add the smallest adapter or service needed by the existing loop; do not add a new model-facing tool unless the four-tool surface cannot express the job.
3. Record durable evidence before deriving summaries or UI state.
4. Add one happy-path test, one interruption test, and one replay/restart test where state survives a turn.
5. Keep the patch reversible until the source note's strongest failure case is proven in the Rust harness.

## Search handles

- Tags: `class/simplification`, `concern/boundary`, `concern/cancellation-timeout`, `concern/concurrency`, `concern/discovery-routing`, `concern/evidence`, `concern/human-control`, `concern/lifecycle`, `domain/build-release`, `domain/extensions`, `domain/llm`, `domain/testing`, `lifecycle/implemented`, `mechanism/policy`
- Aliases: `cancel`, `ReactLoopAgent`, `inject`, `send`, `Agent.send`, `next-turn`, `SendTarget`, `SendOptions`, `dsh-agent`, `user/message`, `Keep agent routing private`, `simplification`, `boundary`, `cancellation timeout`
- Regex: `(?i)(cancel|ReactLoopAgent|inject|send|Agent\.send|next\-turn|SendTarget|SendOptions)`

```bash
rg -n --pcre2 "(?i)(cancel|ReactLoopAgent|inject|send|Agent\\.send|next\\-turn|SendTarget|SendOptions)" source-deepseek-harness
rg -l --fixed-strings "class/simplification" 03-agent-context
```

## Connected notes

- **`source-link`** — [0053. Unify agent delivery on send(target × wakeup) and coalesce injected context into user/message](0053-unify-agent-delivery-on-send-target-wakeup-and-coalesce-injected-context.md): The source note links to this decision directly.
- **`shares-code-with`** — [0464. Request-error retry action](0464-request-error-retry-action.md): Shares source implementation: `packages/core/agent`, `packages/core/agent-loop/src/agent.ts`.
- **`shares-code-with`** — [0653. Stop mirroring the token stream as an agent event](0653-stop-mirroring-the-token-stream-as-an-agent-event.md): Shares source implementation: `packages/core/agent`, `packages/core/agent-loop/src/agent.ts`.
- **`shares-code-with`** — [0449. Stop mirroring durable boundaries as agent events](0449-stop-mirroring-durable-boundaries-as-agent-events.md): Shares source implementation: `packages/core/agent`, `packages/core/agent/README.md`.
- **`shares-code-with`** — [0104. Agent-scoped events dispatch a single payload object](0104-agent-scoped-events-dispatch-a-single-payload-object.md): Shares source implementation: `packages/core/agent`, `packages/core/agent-loop/src/agent.ts`.
- **`shares-code-with`** — [0448. Keep one public stop primitive](0448-keep-one-public-stop-primitive.md): Shares source implementation: `packages/core/agent`, `packages/core/agent/src/index.ts`.
- **`shares-code-with`** — [0038. Initiating Agent scope over AsyncLocalStorage](0038-initiating-agent-scope-over-asynclocalstorage.md): Shares source implementation: `packages/core/agent`, `packages/core/agent/src/index.ts`.
- **`shares-code-with`** — [0011. Agent lifecycle and ownership contracts](0011-agent-lifecycle-and-ownership-contracts.md): Shares source implementation: `packages/core/agent`, `packages/core/agent/src/index.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0469-keep-agent-routing-private.md`.
