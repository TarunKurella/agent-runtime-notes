---
id: "dsh-note-0559"
title: "Ask-user question capability"
status: "archived"
class: "feature"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/archived/feature/2026-06-25-ask-user-question.md"
implementation_evidence: "medium"
target_anchor: "sandbox capability boundary"
tags:
  - "class/feature"
  - "concern/boundary"
  - "concern/cancellation-timeout"
  - "concern/discovery-routing"
  - "concern/human-control"
  - "concern/lifecycle"
  - "concern/ownership"
  - "concern/performance"
  - "concern/recovery"
  - "concern/schema-types"
  - "concern/simplification"
  - "concern/trust"
  - "domain/agent-loop"
  - "domain/extensions"
  - "domain/llm"
  - "domain/protocols"
  - "domain/security"
  - "domain/session-state"
  - "domain/shell-terminal"
  - "domain/testing"
  - "domain/tools"
  - "domain/ui-interaction"
  - "domain/web-retrieval"
  - "lifecycle/archived"
  - "mechanism/event-sourcing"
  - "mechanism/policy"
  - "mechanism/registry"
aliases:
  - "question"
  - "custom"
  - "createApiProxy"
  - "HarnessError"
  - "selected"
  - "label"
  - "desc"
  - "dsh-user-interaction"
  - "ctx.userInteraction"
  - "userInteraction"
  - "dsh-tool-ask-user"
  - "packages/ui"
  - "ask_user_question"
  - "allow_custom"
search_regex: "(?i)(question|custom|createApiProxy|HarnessError|selected|label|desc|dsh\\-user\\-interaction)"
---

# 0559. Ask-user question capability — implementation context

## Open this when

The agent sometimes cannot proceed safely from model inference alone: it needs the human to choose a path, confirm a risky/default action, or provide missing information. Before this change, the only way to get that answer was for the model to ask in assistant text and then stop, which broke the normal tool-call loop: the agent had no structured way to pause, no option metadata for UIs, no abort/error taxonomy, and no way for non-stdio front doors to present the question consistently. This is a user-facing capability, but it also crosses package boundaries.

## Source decision

Introduce dsh-user-interaction as the provider-neutral interface package for ctx.userInteraction, colocated with the model-facing consumer dsh-tool-ask-user under packages/ui. The grouping is intentional: asking a human is a UI-backed product affordance, not part of the providerless core spine. The seam still owns the stable request/answer/error vocabulary, while UI product surfaces provide the concrete provider that collects the answer. The tool registers ask_user_question, forwards { questions, agent, signal }, and returns the provider-computed structured answers as the tool result.

## Decision status

Historical design. Use it to understand an older pressure or failure; locate the replacing decision before implementing anything.

- Raw note: [archived/feature/2026-06-25-ask-user-question.md](../02-notes/archived/feature/2026-06-25-ask-user-question.md)
- Pinned source: [.agents/notes/archived/feature/2026-06-25-ask-user-question.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/archived/feature/2026-06-25-ask-user-question.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`packages/interaction/tool-ask-user/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/interaction/tool-ask-user/src/index.ts) | package entry point | Core file in the package named by the note: `packages/interaction/tool-ask-user`. | `named-package-member` |
| [`packages/interaction/tool-ask-user/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/interaction/tool-ask-user/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/interaction/tool-ask-user`. | `named-package-member` |
| [`packages/core/README.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/README.md) | package contract and examples | Entry point or contract under the directory named by the note: `packages/core`. | `named-directory-member` |
| [`packages/core/agent/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent/src/index.ts) | package entry point | Entry point or contract under the directory named by the note: `packages/core`. | `named-directory-member` |
| [`packages/core/tools/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/index.ts) | package entry point | Entry point or contract under the directory named by the note: `packages/core`. | `named-directory-member` |
| [`packages/core/session/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/index.ts) | package entry point | Entry point or contract under the directory named by the note: `packages/core`. | `named-directory-member` |
| [`packages/core/agent-loop/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent-loop/src/index.ts) | package entry point | Entry point or contract under the directory named by the note: `packages/core`. | `named-directory-member` |
| [`packages/core/system-prompt/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/system-prompt/src/index.ts) | package entry point | Entry point or contract under the directory named by the note: `packages/core`. | `named-directory-member` |
| [`packages/core/agent/src/model-selection.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent/src/model-selection.ts) | runtime implementation | Entry point or contract under the directory named by the note: `packages/core`. | `named-directory-member` |
| [`packages/core`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/packages/core) | package or module directory | The source note names this implementation area directly. | `named-directory` |
| [`packages/interaction/tool-ask-user`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/packages/interaction/tool-ask-user) | package or module directory | The note names this package or capability. | `named-package` |
| [`vendor/cordis/src/utils.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/cordis/src/utils.ts) | runtime implementation | Defines `desc`, a construct named by the note. | `symbol-definition` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `question` | `const` | [`packages/host/apiproxy/src/api-proxy.ts:721`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/src/api-proxy.ts#L721) | `const question = pending.questions[index] as AskUserQuestionItem` |
| `custom` | `const` | [`packages/host/apiproxy/src/api-proxy.ts:724`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/src/api-proxy.ts#L724) | `const custom = answer.custom?.trim()` |
| `createApiProxy` | `function` | [`packages/host/apiproxy/src/api-proxy.ts:1106`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/src/api-proxy.ts#L1106) | `export function createApiProxy(ctx: Context, defaults: ApiProxyDefaults): ApiProxy {` |
| `HarnessError` | `class` | [`packages/llm/llm/src/error.ts:13`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/src/error.ts#L13) | `export class HarnessError extends Error {` |
| `selected` | `const` | [`packages/util/home-paths/src/index.ts:89`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/util/home-paths/src/index.ts#L89) | `const selected = configured ?? (fromEnv !== undefined && fromEnv.trim().length > 0 ? fromEnv : defaultDshHome())` |
| `label` | `const` | [`vendor/cordis/src/events.ts:300`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/cordis/src/events.ts#L300) | `const label = \`ctx.on(${typeof name === 'string' ? JSON.stringify(name) : name.toString()})\`` |
| `desc` | `const` | [`vendor/cordis/src/utils.ts:110`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/cordis/src/utils.ts#L110) | `const desc = Reflect.getOwnPropertyDescriptor(proto, prop)` |

### Tests and executable evidence

- [`packages/llm/llm/tests/service.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/tests/service.spec.ts) — A test under the owning area exercises or imports `HarnessError`.
- [`packages/host/apiproxy/tests/api-proxy-fork.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/tests/api-proxy-fork.spec.ts) — A test under the owning area exercises or imports `createApiProxy`.
- [`packages/host/apiproxy/tests/api-proxy-jobs.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/tests/api-proxy-jobs.spec.ts) — A test under the owning area exercises or imports `createApiProxy`.
- [`packages/host/apiproxy/tests/session-export.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/tests/session-export.spec.ts) — A test under the owning area exercises or imports `createApiProxy`.
- [`packages/host/apiproxy/tests/api-proxy-view.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/tests/api-proxy-view.spec.ts) — A test under the owning area exercises or imports `createApiProxy`.
- [`packages/host/apiproxy/tests/api-proxy-cold.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/tests/api-proxy-cold.spec.ts) — A test under the owning area exercises or imports `createApiProxy`.
- [`packages/host/apiproxy/tests/api-proxy-blank.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/tests/api-proxy-blank.spec.ts) — A test under the owning area exercises or imports `createApiProxy`.
- [`packages/host/apiproxy/tests/api-proxy-rename.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/tests/api-proxy-rename.spec.ts) — A test under the owning area exercises or imports `createApiProxy`.
- Source verification intent: Unit coverage pins provider registration/disposal, duplicate-provider rejection, abort-before-provider, empty-question rejection, structured tool errors through ctx.tools.execute(), batched answers, multi-select answers, custom answers, explicit per-item skips, and the model schema including the removal of value, recommended, allow_custom, and desc. TUI tests cover option descriptions, queued requests, shutdown/abort cleanup, optionless free-form input, invalid choices, duplicate multi-select selections, and batched question flows.

## How to read the implementation

1. Start with [`packages/interaction/tool-ask-user/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/interaction/tool-ask-user/src/index.ts) because it has the strongest evidence link to the note.
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
- **`rust/newtype-ids`** — Represent identity domains with Rust newtypes; do not pass bare strings across session, call, turn, step, and agent boundaries.
- **`rust/provider-adapter`** — Keep provider wire quirks inside adapters; the loop and durable event vocabulary remain provider-neutral.
- **`rust/quickjs-boundary`** — QuickJS holds orchestration variables; Rust/Tokio owns async execution and exposes only explicitly granted capabilities.
- **`rust/serde-validation`** — Parse untrusted input into strict serde types and validate semantic constraints before executing side effects.
- **`rust/tagged-enums`** — Use serde-tagged enums for a closed wire vocabulary. Add an explicit extension envelope only where runtime extension is truly required.
- **`rust/tokio-cancellation`** — Thread one cancellation token through async work, close owned resources before reuse, and test cancellation at every await boundary.
- **`rust/trait-seam`** — Use a small trait at the capability boundary; keep policy and model-facing schemas outside the backend implementation.
- **Status gate** — Historical design. Use it to understand an older pressure or failure; locate the replacing decision before implementing anything.

### Smallest safe implementation slice

1. Express the semantic contract as Rust types and one owning boundary.
2. Add the smallest adapter or service needed by the existing loop; do not add a new model-facing tool unless the four-tool surface cannot express the job.
3. Record durable evidence before deriving summaries or UI state.
4. Add one happy-path test, one interruption test, and one replay/restart test where state survives a turn.
5. Keep the patch reversible until the source note's strongest failure case is proven in the Rust harness.

## Search handles

- Tags: `class/feature`, `concern/boundary`, `concern/cancellation-timeout`, `concern/discovery-routing`, `concern/human-control`, `concern/lifecycle`, `concern/ownership`, `concern/performance`, `concern/recovery`, `concern/schema-types`, `concern/simplification`, `concern/trust`, `domain/agent-loop`, `domain/extensions`, `domain/llm`, `domain/protocols`, `domain/security`, `domain/session-state`, `domain/shell-terminal`, `domain/testing`, `domain/tools`, `domain/ui-interaction`, `domain/web-retrieval`, `lifecycle/archived`, `mechanism/event-sourcing`, `mechanism/policy`, `mechanism/registry`
- Aliases: `question`, `custom`, `createApiProxy`, `HarnessError`, `selected`, `label`, `desc`, `dsh-user-interaction`, `ctx.userInteraction`, `userInteraction`, `dsh-tool-ask-user`, `packages/ui`, `ask_user_question`, `allow_custom`
- Regex: `(?i)(question|custom|createApiProxy|HarnessError|selected|label|desc|dsh\-user\-interaction)`

```bash
rg -n --pcre2 "(?i)(question|custom|createApiProxy|HarnessError|selected|label|desc|dsh\\-user\\-interaction)" source-deepseek-harness
rg -l --fixed-strings "class/feature" 03-agent-context
```

## Connected notes

- **`shares-code-with`** — [0561. Plan mode --- a logged per-agent session mode](0561-plan-mode-a-logged-per-agent-session-mode.md): Shares source implementation: `packages/core/agent/src/index.ts`, `packages/core/session/src/index.ts`.
- **`shares-code-with`** — [0159. Fresh-agent Ralph workflow tool](0159-fresh-agent-ralph-workflow-tool.md): Shares source implementation: `packages/core/agent-loop/src/index.ts`, `packages/core/agent/src/index.ts`.
- **`shares-code-with`** — [0134. Subagent capability seam](0134-subagent-capability-seam.md): Shares source implementation: `packages/core/agent-loop/src/index.ts`, `packages/core/agent/src/index.ts`.
- **`shares-code-with`** — [0158. persistent PTY sessions](0158-persistent-pty-sessions.md): Shares source implementation: `packages/core/agent/src/index.ts`, `packages/core/tools/src/index.ts`.
- **`shares-code-with`** — [0511. Session projections and command lifecycle logging](0511-session-projections-and-command-lifecycle-logging.md): Shares source implementation: `packages/core/session/src/index.ts`, `packages/core/tools/src/index.ts`.
- **`shares-code-with`** — [0146. The subprocess sandbox --- confinement seam, native runners, escalation, and per-session modes](0146-the-subprocess-sandbox-confinement-seam-native-runners-escalation-and-pe.md): Shares source implementation: `packages/core/agent-loop/src/index.ts`, `packages/core/agent/src/index.ts`.
- **`shares-code-with`** — [0035. Agent-scope runtime design and correctness](0035-agent-scope-runtime-design-and-correctness.md): Shares source implementation: `packages/core/agent/src/index.ts`, `packages/core/session/src/index.ts`.
- **`shares-code-with`** — [0334. Reject human interaction from runtime-owned subagents](0334-reject-human-interaction-from-runtime-owned-subagents.md): Shares source implementation: `packages/core/agent/src/index.ts`, `packages/interaction/tool-ask-user/src/index.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0559-ask-user-question-capability.md`.
