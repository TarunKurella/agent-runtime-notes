---
id: "dsh-note-0334"
title: "Reject human interaction from runtime-owned subagents"
status: "implemented"
class: "bug-fix"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/bug-fix/2026-08-01-ask-user-delegated-caller-guard.md"
implementation_evidence: "medium"
target_anchor: "Rust provider-adapter boundary"
tags:
  - "class/bug-fix"
  - "concern/boundary"
  - "concern/discovery-routing"
  - "concern/human-control"
  - "concern/ownership"
  - "concern/performance"
  - "concern/recovery"
  - "concern/schema-types"
  - "domain/llm"
  - "domain/multi-agent"
  - "domain/protocols"
  - "domain/session-state"
  - "domain/storage"
  - "domain/testing"
  - "domain/tools"
  - "domain/ui-interaction"
  - "domain/web-retrieval"
  - "lifecycle/implemented"
  - "mechanism/policy"
  - "mechanism/registry"
aliases:
  - "agents"
  - "ask_user_question"
  - "exit_plan_mode"
  - "ctx.userQuestions.ask"
  - "AskUserQuestionRequest.agent"
  - "UserQuestionService.ask"
  - "ctx.agents"
  - "ctx.agents.roots"
  - "CALLER_NOT_LIVE"
  - "DELEGATED_CALLER"
  - "delegationDepth"
  - "session.header.delegationDepth > 0"
  - "dsh-tool-ask-user"
  - "Reject human interaction from runtime-owned subagents"
search_regex: "(?i)(agents|ask_user_question|exit_plan_mode|ctx\\.userQuestions\\.ask|AskUserQuestionRequest\\.agent|UserQuestionService\\.ask|ctx\\.agents|ctx\\.agents\\.roots)"
---

# 0334. Reject human interaction from runtime-owned subagents — implementation context

## Open this when

A one-shot subagent that calls ask_user_question can block indefinitely. The call waits for a human answer, but the child has no independently owned human channel, so the child's completion and the parent waiting on that completion both stall. Durable session lineage cannot decide whether an answerer exists. A child session may later be resumed as a new top-level runtime root, while a live runtime-owned child may carry a zero or absent durable delegation depth. Error guidance at the shared seam must also fit every consumer: exit_plan_mode uses ctx.userQuestions.ask() without calling ask_user_question.

## Source decision

When AskUserQuestionRequest.agent is present, UserQuestionService.ask() authenticates the exact live agent through ctx.agents and admits it only when ctx.agents.roots() contains that instance. A missing registry or stale same-id object fails with CALLER_NOT_LIVE; a live agent owned by another live agent fails with DELEGATED_CALLER. The check runs after the existing aborted and empty-batch guards and before intent validation or provider dispatch, so an owned child never creates a UI wait. Runtime ownership is the authority.

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/bug-fix/2026-08-01-ask-user-delegated-caller-guard.md](../02-notes/implemented/bug-fix/2026-08-01-ask-user-delegated-caller-guard.md)
- Pinned source: [.agents/notes/implemented/bug-fix/2026-08-01-ask-user-delegated-caller-guard.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/bug-fix/2026-08-01-ask-user-delegated-caller-guard.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`packages/core/agent/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent/src/index.ts) | package entry point | Core file in the package named by the note: `packages/core/agent`. | `named-package-member` |
| [`packages/core/agent/src/types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent/src/types.ts) | public types and contract | Core file in the package named by the note: `packages/core/agent`. | `named-package-member` |
| [`packages/core/agent/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/core/agent`. | `named-package-member` |
| [`packages/interaction/tool-ask-user/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/interaction/tool-ask-user/src/index.ts) | package entry point | Core file in the package named by the note: `packages/interaction/tool-ask-user`. | `named-package-member` |
| [`packages/interaction/tool-ask-user/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/interaction/tool-ask-user/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/interaction/tool-ask-user`. | `named-package-member` |
| [`packages/core/agent`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent) | package or module directory | The note names this package or capability. | `named-package` |
| [`packages/interaction/tool-ask-user`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/packages/interaction/tool-ask-user) | package or module directory | The note names this package or capability. | `named-package` |
| [`packages/acp/acp/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/acp/acp/src/index.ts) | package entry point | Defines `agents`, a construct named by the note. | `symbol-definition` |
| [`packages/core/agent/README.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent/README.md) | package contract and examples | Core file in the package named by the note: `packages/core/agent`. | `named-package-member` |
| [`packages/core/agent/package.json`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent/package.json) | composition and configuration | Core file in the package named by the note: `packages/core/agent`. | `named-package-member` |
| [`packages/interaction/tool-ask-user/README.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/interaction/tool-ask-user/README.md) | package contract and examples | Core file in the package named by the note: `packages/interaction/tool-ask-user`. | `named-package-member` |
| [`packages/interaction/tool-ask-user/package.json`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/interaction/tool-ask-user/package.json) | composition and configuration | Core file in the package named by the note: `packages/interaction/tool-ask-user`. | `named-package-member` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `agents` | `const` | [`packages/acp/acp/src/index.ts:108`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/acp/acp/src/index.ts#L108) | `const agents = ctx.agents` |

### Tests and executable evidence

- [`packages/interaction/tool-ask-user/tests/tool-ask-user.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/interaction/tool-ask-user/tests/tool-ask-user.spec.ts) — A test under the owning area exercises or imports `ask_user_question`. A test under the owning area exercises or imports `DELEGATED_CALLER`.
- Source verification intent: Service tests cover a zero-depth live child, a depth-one resumed runtime root, a missing registry, a stale same-id object, and provider non-invocation on every rejection. Tool and plan-mode tests prove both consumers surface the neutral DELEGATED_CALLER result and never reach the provider. The keyless assembled snapshot delegates to a child that attempts ask_user_question, pins the child's error tool result and final handoff, and proves the parent completes instead of waiting for an answer.

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

- **`rust/append-log`** — Keep durable facts append-only and derive views from them. Separate the synchronous fact boundary from asynchronous persistence.
- **`rust/four-tool-surface`** — Preserve the four model tools: read, write, patch, and exec. Reuse internal capabilities without automatically exposing new model-facing tools.
- **`rust/newtype-ids`** — Represent identity domains with Rust newtypes; do not pass bare strings across session, call, turn, step, and agent boundaries.
- **`rust/provider-adapter`** — Keep provider wire quirks inside adapters; the loop and durable event vocabulary remain provider-neutral.
- **`rust/serde-validation`** — Parse untrusted input into strict serde types and validate semantic constraints before executing side effects.
- **`rust/tagged-enums`** — Use serde-tagged enums for a closed wire vocabulary. Add an explicit extension envelope only where runtime extension is truly required.
- **`rust/trait-seam`** — Use a small trait at the capability boundary; keep policy and model-facing schemas outside the backend implementation.

### Smallest safe implementation slice

1. Express the semantic contract as Rust types and one owning boundary.
2. Add the smallest adapter or service needed by the existing loop; do not add a new model-facing tool unless the four-tool surface cannot express the job.
3. Record durable evidence before deriving summaries or UI state.
4. Add one happy-path test, one interruption test, and one replay/restart test where state survives a turn.
5. Keep the patch reversible until the source note's strongest failure case is proven in the Rust harness.

## Search handles

- Tags: `class/bug-fix`, `concern/boundary`, `concern/discovery-routing`, `concern/human-control`, `concern/ownership`, `concern/performance`, `concern/recovery`, `concern/schema-types`, `domain/llm`, `domain/multi-agent`, `domain/protocols`, `domain/session-state`, `domain/storage`, `domain/testing`, `domain/tools`, `domain/ui-interaction`, `domain/web-retrieval`, `lifecycle/implemented`, `mechanism/policy`, `mechanism/registry`
- Aliases: `agents`, `ask_user_question`, `exit_plan_mode`, `ctx.userQuestions.ask`, `AskUserQuestionRequest.agent`, `UserQuestionService.ask`, `ctx.agents`, `ctx.agents.roots`, `CALLER_NOT_LIVE`, `DELEGATED_CALLER`, `delegationDepth`, `session.header.delegationDepth > 0`, `dsh-tool-ask-user`, `Reject human interaction from runtime-owned subagents`
- Regex: `(?i)(agents|ask_user_question|exit_plan_mode|ctx\.userQuestions\.ask|AskUserQuestionRequest\.agent|UserQuestionService\.ask|ctx\.agents|ctx\.agents\.roots)`

```bash
rg -n --pcre2 "(?i)(agents|ask_user_question|exit_plan_mode|ctx\\.userQuestions\\.ask|AskUserQuestionRequest\\.agent|UserQuestionService\\.ask|ctx\\.agents|ctx\\.agents\\.roots)" source-deepseek-harness
rg -l --fixed-strings "class/bug-fix" 03-agent-context
```

## Connected notes

- **`source-link`** — [0515. Semantic phases for composer-chain election](0515-semantic-phases-for-composer-chain-election.md): The source note links to this decision directly.
- **`shares-code-with`** — [0449. Stop mirroring durable boundaries as agent events](0449-stop-mirroring-durable-boundaries-as-agent-events.md): Shares source implementation: `packages/acp/acp/src/index.ts`, `packages/core/agent`.
- **`shares-code-with`** — [0464. Request-error retry action](0464-request-error-retry-action.md): Shares source implementation: `packages/core/agent`, `packages/core/agent/README.md`.
- **`shares-code-with`** — [0450. Unify the agent id and the session id](0450-unify-the-agent-id-and-the-session-id.md): Shares source implementation: `packages/acp/acp/src/index.ts`, `packages/core/agent`.
- **`shares-code-with`** — [0653. Stop mirroring the token stream as an agent event](0653-stop-mirroring-the-token-stream-as-an-agent-event.md): Shares source implementation: `packages/core/agent`, `packages/core/agent/README.md`.
- **`shares-code-with`** — [0469. Keep agent routing private](0469-keep-agent-routing-private.md): Shares source implementation: `packages/core/agent`, `packages/core/agent/README.md`.
- **`shares-code-with`** — [0557. Agent Client Protocol (ACP) support --- drive the coding agent from external editors](0557-agent-client-protocol-acp-support-drive-the-coding-agent-from-external-e.md): Shares source implementation: `packages/acp/acp/src/index.ts`, `packages/core/agent/src/index.ts`.
- **`shares-code-with`** — [0144. The approval seam --- one-shot permission decisions over a waterfall of answerers](0144-the-approval-seam-one-shot-permission-decisions-over-a-waterfall-of-answ.md): Shares source implementation: `packages/acp/acp/src/index.ts`, `packages/core/agent/src/index.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0334-reject-human-interaction-from-runtime-owned-subagents.md`.
