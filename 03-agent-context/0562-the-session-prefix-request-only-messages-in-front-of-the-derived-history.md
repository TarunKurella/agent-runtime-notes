---
id: "dsh-note-0562"
title: "The session prefix --- request-only messages in front of the derived history"
status: "archived"
class: "feature"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/archived/feature/2026-07-07-session-prefix.md"
implementation_evidence: "high"
target_anchor: "read/write/patch filesystem capability"
tags:
  - "class/feature"
  - "concern/boundary"
  - "concern/cancellation-timeout"
  - "concern/discovery-routing"
  - "concern/evidence"
  - "concern/lifecycle"
  - "concern/performance"
  - "concern/recovery"
  - "domain/agent-loop"
  - "domain/build-release"
  - "domain/context"
  - "domain/extensions"
  - "domain/filesystem"
  - "domain/llm"
  - "domain/session-state"
  - "domain/storage"
  - "domain/testing"
  - "lifecycle/archived"
  - "mechanism/event-sourcing"
  - "mechanism/registry"
aliases:
  - "after"
  - "before"
  - "resume"
  - "system"
  - "inject"
  - "additionalContexts"
  - "messages"
  - "user/message"
  - "agent/step"
  - "<system-reminder>"
  - "agent.inject"
  - "context/message"
  - "deriveMessages"
  - "agent/session-prefix"
search_regex: "(?i)(after|before|resume|system|inject|additionalContexts|messages|user/message)"
---

# 0562. The session prefix --- request-only messages in front of the derived history — implementation context

## Open this when

A plugin often owns a session-stable opener the model must always see --- a skills catalog, an AGENTS.md digest, a workspace baseline. Before this seam the harness offered two homes, and both are wrong for that content. The system prompt is one rendered string: message-shaped content (a user-role envelope, a multi-message primer) does not fit it, and providers weight conversation messages differently from system text.

## Source decision

agent/session-prefix is a waterfall on the agent event map (packages/core/agent/src/types.ts): listeners receive a frozen empty seed and return an extension (the canonical contribution is a prepend, [mine, ...await next()], which yields registration order on the wire). The loop (agent-loop source) fires it once per loop instance, lazily before the instance's first agent/pre-step; the composed list is deep-cloned, deep-frozen, cached on the instance, and placed in front of the ENTIRE derived history --- directly after the provider's system slot --- on every request the instance sends (wire order).

## Decision status

Historical design. Use it to understand an older pressure or failure; locate the replacing decision before implementing anything.

- Raw note: [archived/feature/2026-07-07-session-prefix.md](../02-notes/archived/feature/2026-07-07-session-prefix.md)
- Pinned source: [.agents/notes/archived/feature/2026-07-07-session-prefix.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/archived/feature/2026-07-07-session-prefix.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`packages/core/agent/src/types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent/src/types.ts) | public types and contract | The source note names this file directly. | `named-file` |
| [`packages/core/agent-loop/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent-loop/src/invariant.ts) | runtime contract checks | Entry point or contract under the directory named by the note: `packages/core/agent-loop/src`. The source note names this file directly. | `named-directory-member, named-file, named-package-member, symbol-definition` |
| [`packages/core/agent-loop/src/agent.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent-loop/src/agent.ts) | runtime implementation | Entry point or contract under the directory named by the note: `packages/core/agent-loop/src`. Core file in the package named by the note: `packages/core/agent-loop`. | `named-directory-member, named-package-member, symbol-definition` |
| [`packages/core/agent-loop/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent-loop/src/index.ts) | package entry point | Core file in the package named by the note: `packages/core/agent-loop`. | `named-package-member` |
| [`packages/core/agent-loop/src`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent-loop/src) | package or module directory | The source note names this implementation area directly. | `named-directory` |
| [`packages/core/agent-loop`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent-loop) | package or module directory | The note names this package or capability. | `named-package` |
| [`apps/cli/src/plugin.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/cli/src/plugin.ts) | runtime implementation | Defines `before`, a construct named by the note. Defines `after`, a construct named by the note. | `symbol-definition` |
| [`packages/llm/llm/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/src/index.ts) | package entry point | Defines `messages`, a construct named by the note. | `symbol-definition` |
| [`packages/core/tools/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/index.ts) | package entry point | Defines `additionalContexts`, a construct named by the note. | `symbol-definition` |
| [`packages/api/remotes/src/agent-lookup.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/api/remotes/src/agent-lookup.ts) | runtime implementation | Defines `resume`, a construct named by the note. | `symbol-definition` |
| [`packages/core/agent-loop/README.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent-loop/README.md) | package contract and examples | Core file in the package named by the note: `packages/core/agent-loop`. | `named-package-member` |
| [`packages/core/agent-loop/package.json`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent-loop/package.json) | composition and configuration | Core file in the package named by the note: `packages/core/agent-loop`. | `named-package-member` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `after` | `const` | [`apps/cli/src/plugin.ts:60`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/cli/src/plugin.ts#L60) | `const after = readProfileManifest(NAME, profileDir)` |
| `before` | `const` | [`apps/cli/src/plugin.ts:126`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/cli/src/plugin.ts#L126) | `const before = readProfileManifest(NAME, dir)` |
| `resume` | `let` | [`packages/api/remotes/src/agent-lookup.ts:143`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/api/remotes/src/agent-lookup.ts#L143) | `let resume = resumes.get(sessionId)` |
| `system` | `const` | [`packages/core/agent-loop/src/agent.ts:337`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent-loop/src/agent.ts#L337) | `const system = renderPrompt(assembly)` |
| `inject` | `const` | [`packages/core/agent-loop/src/invariant.ts:16`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent-loop/src/invariant.ts#L16) | `export const inject = ['invariants']` |
| `additionalContexts` | `const` | [`packages/core/tools/src/index.ts:1760`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/index.ts#L1760) | `const additionalContexts = [` |
| `messages` | `const` | [`packages/llm/llm/src/index.ts:824`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/src/index.ts#L824) | `const messages: Message[] = options.messages.map((message) => {` |

### Tests and executable evidence

- [`packages/core/agent-loop/tests/cancel.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent-loop/tests/cancel.spec.ts) — The source note names this file directly.
- [`packages/core/agent-loop/tests/interception.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent-loop/tests/interception.spec.ts) — The source note names this file directly. A test under the owning area exercises or imports `additionalContexts`.
- [`packages/core/agent-loop/tests/request-cache.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent-loop/tests/request-cache.e2e.ts) — The source note names this file directly. A test under the owning area exercises or imports `deriveMessages`.
- [`packages/core/tools/tests/tools.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/tests/tools.spec.ts) — A test under the owning area exercises or imports `additionalContexts`.
- [`packages/core/agent-loop/tests/loop.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent-loop/tests/loop.spec.ts) — A test under the owning area exercises or imports `deriveMessages`.
- [`packages/core/tools/tests/code-mode.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/tests/code-mode.spec.ts) — A test under the owning area exercises or imports `additionalContexts`.
- [`packages/core/agent-loop/tests/resume.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent-loop/tests/resume.spec.ts) — A test under the owning area exercises or imports `deriveMessages`.
- [`packages/core/agent-loop/tests/invariant.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent-loop/tests/invariant.spec.ts) — A test under the owning area exercises or imports `deriveMessages`.
- Source verification intent: Interception tests pin compose-once reuse without changed headers, prepend order, empty-prefix omission, immutability, composition before pre-step, and the prefix on the routed header; cancellation tests pin discard and recomposition. Session, invariant, token-meter, and compaction tests cover header round trips, request reconstruction, and durable prefix-aware pressure accounting. Snapshot normalization preserves prefix counts, while the pinned-header scenario owns content and the default example remains prefix-free.

## How to read the implementation

1. Start with [`packages/core/agent/src/types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent/src/types.ts) because it has the strongest evidence link to the note.
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
- **`rust/process-discipline`** — Treat this as repository or release policy unless it changes a runtime contract. Do not add runtime machinery for a process-only lesson.
- **`rust/provider-adapter`** — Keep provider wire quirks inside adapters; the loop and durable event vocabulary remain provider-neutral.
- **`rust/quickjs-boundary`** — QuickJS holds orchestration variables; Rust/Tokio owns async execution and exposes only explicitly granted capabilities.
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

- Tags: `class/feature`, `concern/boundary`, `concern/cancellation-timeout`, `concern/discovery-routing`, `concern/evidence`, `concern/lifecycle`, `concern/performance`, `concern/recovery`, `domain/agent-loop`, `domain/build-release`, `domain/context`, `domain/extensions`, `domain/filesystem`, `domain/llm`, `domain/session-state`, `domain/storage`, `domain/testing`, `lifecycle/archived`, `mechanism/event-sourcing`, `mechanism/registry`
- Aliases: `after`, `before`, `resume`, `system`, `inject`, `additionalContexts`, `messages`, `user/message`, `agent/step`, `<system-reminder>`, `agent.inject`, `context/message`, `deriveMessages`, `agent/session-prefix`
- Regex: `(?i)(after|before|resume|system|inject|additionalContexts|messages|user/message)`

```bash
rg -n --pcre2 "(?i)(after|before|resume|system|inject|additionalContexts|messages|user/message)" source-deepseek-harness
rg -l --fixed-strings "class/feature" 03-agent-context
```

## Connected notes

- **`source-link`** — [0681. Pin request-header content in one snapshot scenario](0681-pin-request-header-content-in-one-snapshot-scenario.md): The source note links to this decision directly.
- **`shares-code-with`** — [0146. The subprocess sandbox --- confinement seam, native runners, escalation, and per-session modes](0146-the-subprocess-sandbox-confinement-seam-native-runners-escalation-and-pe.md): Shares source implementation: `packages/core/agent-loop/src/agent.ts`, `packages/core/agent-loop/src/index.ts`.
- **`shares-code-with`** — [0004. Microkernel --- extension via Cordis event taxonomy, one concrete loop](0004-microkernel-extension-via-cordis-event-taxonomy-one-concrete-loop.md): Shares source implementation: `packages/core/agent-loop`, `packages/core/agent-loop/src/index.ts`.
- **`shares-code-with`** — [0563. Repeat-tool-call guard plugin](0563-repeat-tool-call-guard-plugin.md): Shares source implementation: `packages/core/agent-loop/src/index.ts`, `packages/core/agent-loop/src/invariant.ts`.
- **`shares-code-with`** — [0162. Persisted same-session goal domain](0162-persisted-same-session-goal-domain.md): Shares source implementation: `packages/core/agent-loop/src/index.ts`, `packages/core/agent-loop/src/invariant.ts`.
- **`shares-code-with`** — [0305. Semantic session checkpoints](0305-semantic-session-checkpoints.md): Shares source implementation: `packages/core/agent-loop`, `packages/core/agent-loop/src/index.ts`.
- **`shares-code-with`** — [0335. Goal-round wrap-up message](0335-goal-round-wrap-up-message.md): Shares source implementation: `packages/core/agent-loop`, `packages/core/agent-loop/src/index.ts`.
- **`shares-code-with`** — [0290. Background job completion wakes an idle owner](0290-background-job-completion-wakes-an-idle-owner.md): Shares source implementation: `packages/core/agent-loop/src/agent.ts`, `packages/core/agent-loop/src/index.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0562-the-session-prefix-request-only-messages-in-front-of-the-derived-history.md`.
