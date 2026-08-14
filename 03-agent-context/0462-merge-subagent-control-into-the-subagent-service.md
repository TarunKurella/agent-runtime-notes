---
id: "dsh-note-0462"
title: "Merge subagent control into the subagent service"
status: "implemented"
class: "simplification"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/simplification/2026-07-26-merge-subagent-control-service.md"
implementation_evidence: "medium"
target_anchor: "Rust provider-adapter boundary"
tags:
  - "class/simplification"
  - "concern/boundary"
  - "concern/cancellation-timeout"
  - "concern/discovery-routing"
  - "concern/human-control"
  - "concern/lifecycle"
  - "concern/ownership"
  - "concern/recovery"
  - "domain/build-release"
  - "domain/extensions"
  - "domain/llm"
  - "domain/multi-agent"
  - "domain/session-state"
  - "domain/storage"
  - "lifecycle/implemented"
  - "mechanism/adapter"
  - "mechanism/policy"
  - "mechanism/registry"
aliases:
  - "subagents"
  - "resume"
  - "subagent"
  - "SubagentError"
  - "SubagentRuntime"
  - "ctx.subagentControl"
  - "subagentControl"
  - "ctx.subagents"
  - "provider.resume"
  - "send_message"
  - "startContinuable"
  - "@deepseek-ai/dsh-subagent-control"
  - "@deepseek-ai/dsh-tool-subagent-control"
  - "ctx.inject"
search_regex: "(?i)(subagents|resume|subagent|SubagentError|SubagentRuntime|ctx\\.subagentControl|subagentControl|ctx\\.subagents)"
---

# 0462. Merge subagent control into the subagent service — implementation context

## Open this when

Continuable-child orchestration originally lived in a separate ctx.subagentControl service above the raw ctx.subagents provider contract. That split kept provider dispatch independent of Jobs and persistence, and gave model and human adapters one orchestration contract. In practice the two services described one capability family, every continuable caller needed both, and the provider-bound delegation tool had to infer policy from provider.resume and inspect whether the control service and send_message tool happened to be loaded.

## Source decision

SubagentRuntime is the only public service. It exposes ordinary start(name, request), Task-backed startContinuable(spec), and intent-named followup(...); provider resume dispatch remains private to its continuation manager. The standalone @deepseek-ai/dsh-subagent-control package and ctx.subagentControl key are absent; the optional @deepseek-ai/dsh-tool-subagent-control package injects ctx.subagents directly. The merged service and its providers expose one SubagentError taxonomy.

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/simplification/2026-07-26-merge-subagent-control-service.md](../02-notes/implemented/simplification/2026-07-26-merge-subagent-control-service.md)
- Pinned source: [.agents/notes/implemented/simplification/2026-07-26-merge-subagent-control-service.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/simplification/2026-07-26-merge-subagent-control-service.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`packages/subagent/subagent/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/subagent/subagent/src/index.ts) | package entry point | Core file in the package named by the note: `packages/subagent/subagent`. Defines `SubagentRuntime`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/subagent/subagent/src/types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/subagent/subagent/src/types.ts) | public types and contract | Core file in the package named by the note: `packages/subagent/subagent`. | `named-package-member` |
| [`packages/subagent/subagent/src/error.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/subagent/subagent/src/error.ts) | runtime implementation | Core file in the package named by the note: `packages/subagent/subagent`. Defines `SubagentError`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/subagent/subagent/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/subagent/subagent/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/subagent/subagent`. | `named-package-member` |
| [`packages/subagent/tool-subagent/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/subagent/tool-subagent/src/index.ts) | package entry point | Core file in the package named by the note: `packages/subagent/tool-subagent`. | `named-package-member` |
| [`packages/subagent/tool-subagent/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/subagent/tool-subagent/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/subagent/tool-subagent`. | `named-package-member` |
| [`packages/subagent/tool-subagent-control/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/subagent/tool-subagent-control/src/index.ts) | package entry point | Core file in the package named by the note: `packages/subagent/tool-subagent-control`. | `named-package-member` |
| [`packages/subagent/tool-subagent-control/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/subagent/tool-subagent-control/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/subagent/tool-subagent-control`. | `named-package-member` |
| [`packages/subagent/subagent`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/packages/subagent/subagent) | package or module directory | The note names this package or capability. | `named-package` |
| [`packages/subagent/tool-subagent`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/packages/subagent/tool-subagent) | package or module directory | The note names this package or capability. | `named-package` |
| [`packages/subagent/tool-subagent-control`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/packages/subagent/tool-subagent-control) | package or module directory | The note names this package or capability. | `named-package` |
| [`packages/acp/acp/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/acp/acp/src/index.ts) | package entry point | Defines `subagents`, a construct named by the note. | `symbol-definition` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `subagents` | `const` | [`packages/acp/acp/src/index.ts:376`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/acp/acp/src/index.ts#L376) | `const subagents = ctx.get('subagents') as ContinuableDrain \| undefined` |
| `resume` | `let` | [`packages/api/remotes/src/agent-lookup.ts:143`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/api/remotes/src/agent-lookup.ts#L143) | `let resume = resumes.get(sessionId)` |
| `subagent` | `const` | [`packages/client/ui-subagent/src/client/index.ts:45`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-subagent/src/client/index.ts#L45) | `const subagent = owner.session?.subagent` |
| `SubagentError` | `class` | [`packages/subagent/subagent/src/error.ts:10`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/subagent/subagent/src/error.ts#L10) | `export class SubagentError extends HarnessError {` |
| `SubagentRuntime` | `class` | [`packages/subagent/subagent/src/index.ts:171`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/subagent/subagent/src/index.ts#L171) | `export class SubagentRuntime extends Service {` |

### Tests and executable evidence

- [`packages/subagent/subagent/tests/service.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/subagent/subagent/tests/service.spec.ts) — A test under the owning area exercises or imports `startContinuable`. A test under the owning area exercises or imports `SubagentError`.
- [`packages/subagent/subagent/tests/continuation.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/subagent/subagent/tests/continuation.spec.ts) — A test under the owning area exercises or imports `startContinuable`. A test under the owning area exercises or imports `SubagentError`.
- [`packages/subagent/subagent/tests/list-children.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/subagent/subagent/tests/list-children.spec.ts) — A test under the owning area exercises or imports `startContinuable`. A test under the owning area exercises or imports `SubagentError`.
- [`packages/subagent/tool-subagent/tests/tool-subagent.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/subagent/tool-subagent/tests/tool-subagent.spec.ts) — A test under the owning area exercises or imports `send_message`. A test under the owning area exercises or imports `SubagentRuntime`.
- [`packages/subagent/tool-subagent/tests/scripted-provider.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/subagent/tool-subagent/tests/scripted-provider.spec.ts) — A test under the owning area exercises or imports `SubagentRuntime`.
- [`packages/subagent/tool-subagent-control/tests/list-agents.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/subagent/tool-subagent-control/tests/list-agents.spec.ts) — A test under the owning area exercises or imports `send_message`. A test under the owning area exercises or imports `SubagentRuntime`.
- [`packages/subagent/subagent/tests/continuation-inheritance.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/subagent/subagent/tests/continuation-inheritance.spec.ts) — A test under the owning area exercises or imports `startContinuable`.
- [`packages/subagent/tool-subagent-control/tests/tool-subagent-control.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/subagent/tool-subagent-control/tests/tool-subagent-control.spec.ts) — A test under the owning area exercises or imports `send_message`. A test under the owning area exercises or imports `SubagentRuntime`.

## How to read the implementation

1. Start with [`packages/subagent/subagent/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/subagent/subagent/src/index.ts) because it has the strongest evidence link to the note.
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
- **`rust/tokio-cancellation`** — Thread one cancellation token through async work, close owned resources before reuse, and test cancellation at every await boundary.
- **`rust/trait-seam`** — Use a small trait at the capability boundary; keep policy and model-facing schemas outside the backend implementation.

### Smallest safe implementation slice

1. Express the semantic contract as Rust types and one owning boundary.
2. Add the smallest adapter or service needed by the existing loop; do not add a new model-facing tool unless the four-tool surface cannot express the job.
3. Record durable evidence before deriving summaries or UI state.
4. Add one happy-path test, one interruption test, and one replay/restart test where state survives a turn.
5. Keep the patch reversible until the source note's strongest failure case is proven in the Rust harness.

## Search handles

- Tags: `class/simplification`, `concern/boundary`, `concern/cancellation-timeout`, `concern/discovery-routing`, `concern/human-control`, `concern/lifecycle`, `concern/ownership`, `concern/recovery`, `domain/build-release`, `domain/extensions`, `domain/llm`, `domain/multi-agent`, `domain/session-state`, `domain/storage`, `lifecycle/implemented`, `mechanism/adapter`, `mechanism/policy`, `mechanism/registry`
- Aliases: `subagents`, `resume`, `subagent`, `SubagentError`, `SubagentRuntime`, `ctx.subagentControl`, `subagentControl`, `ctx.subagents`, `provider.resume`, `send_message`, `startContinuable`, `@deepseek-ai/dsh-subagent-control`, `@deepseek-ai/dsh-tool-subagent-control`, `ctx.inject`
- Regex: `(?i)(subagents|resume|subagent|SubagentError|SubagentRuntime|ctx\.subagentControl|subagentControl|ctx\.subagents)`

```bash
rg -n --pcre2 "(?i)(subagents|resume|subagent|SubagentError|SubagentRuntime|ctx\\.subagentControl|subagentControl|ctx\\.subagents)" source-deepseek-harness
rg -l --fixed-strings "class/simplification" 03-agent-context
```

## Connected notes

- **`source-link`** — [0200. Continuable subagents](0200-continuable-subagents.md): The source note links to this decision directly.
- **`source-link`** — [0463. Intent-named subagent continuation operations](0463-intent-named-subagent-continuation-operations.md): The source note links to this decision directly.
- **`shares-code-with`** — [0148. Background subagent tasks](0148-background-subagent-tasks.md): Shares source implementation: `packages/subagent/subagent`, `packages/subagent/subagent/src/index.ts`.
- **`shares-code-with`** — [0280. Parallel subagent delegations](0280-parallel-subagent-delegations.md): Shares source implementation: `packages/subagent/subagent/src/index.ts`, `packages/subagent/subagent/src/invariant.ts`.
- **`shares-code-with`** — [0153. Configure subagent persona, tool visibility, and depth](0153-configure-subagent-persona-tool-visibility-and-depth.md): Shares source implementation: `packages/subagent/subagent/src/index.ts`, `packages/subagent/subagent/src/types.ts`.
- **`shares-code-with`** — [0184. In-process subagent policy inheritance --- the child starts under the parent's sandbox override](0184-in-process-subagent-policy-inheritance-the-child-starts-under-the-parent.md): Shares source implementation: `packages/subagent/subagent`, `packages/subagent/subagent/src/index.ts`.
- **`shares-code-with`** — [0285. Delegated subagents run with approvals pinned to `'never'`](0285-delegated-subagents-run-with-approvals-pinned-to-never.md): Shares source implementation: `packages/subagent/subagent`, `packages/subagent/subagent/src/index.ts`.
- **`shares-code-with`** — [0664. Retire the standalone subagent mock package](0664-retire-the-standalone-subagent-mock-package.md): Shares source implementation: `packages/subagent/subagent/src/types.ts`, `packages/subagent/tool-subagent`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0462-merge-subagent-control-into-the-subagent-service.md`.
