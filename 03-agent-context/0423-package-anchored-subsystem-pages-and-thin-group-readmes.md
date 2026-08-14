---
id: "dsh-note-0423"
title: "Package-anchored subsystem pages and thin group READMEs"
status: "implemented"
class: "process"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/process/2026-08-03-package-anchored-subsystem-pages.md"
implementation_evidence: "high"
target_anchor: "repository tests and engineering policy"
tags:
  - "class/process"
  - "concern/boundary"
  - "concern/cancellation-timeout"
  - "concern/discovery-routing"
  - "concern/evidence"
  - "concern/ownership"
  - "concern/recovery"
  - "domain/build-release"
  - "domain/llm"
  - "domain/session-state"
  - "domain/testing"
  - "lifecycle/implemented"
  - "mechanism/policy"
  - "mechanism/projection"
aliases:
  - "CreateAgentOptions"
  - "ResumeAgentOptions"
  - "AgentHandle"
  - "AgentFactory"
  - "SessionEvent"
  - "TYPE_LINK_EXEMPTIONS"
  - "docs/subsystems/"
  - "packages/core"
  - "packages/llm"
  - "…Map → derived-union"
  - "LINK_MAP → core.md"
  - "packages/<group>/README.md"
  - "scripts/project-doc-site.spec.ts"
  - "packages/core/agent"
search_regex: "(?i)(CreateAgentOptions|ResumeAgentOptions|AgentHandle|AgentFactory|SessionEvent|TYPE_LINK_EXEMPTIONS|docs/subsystems/|packages/core)"
---

# 0423. Package-anchored subsystem pages and thin group READMEs — implementation context

## Open this when

The subsystems catalog scoped its front page by the spine-vs-seam rule: a type was "core" if the loop holds, derives, streams, or logs it on every turn. That rule selected types, not packages, so as the folder grew to forty-plus pages the front page became a cross-package grab-bag: LLM conversation vocabulary sat above the agent contracts, the creation/ownership vocabulary (AgentHandle, CreateAgentOptions, ResumeAgentOptions, AgentFactory) was documented nowhere in the folder because the generator exempted it to a package README, and a reader could not predict which page documents a type from where the type.

## Source decision

Every docs/subsystems/ page anchors to the package or package group that declares its vocabulary, and page membership follows the repository layout: core.md is the packages/core page (creation and ownership, the Agent handle with its delivery/cancellation/interception contracts, pointers to the group's dedicated pages), llm-streaming.md owns packages/llm end-to-end, and so on. Repo-wide type patterns (…Map → derived-union, branded ids) stay on core.md in an explicitly framed closing section rather than interleaved with the package content.

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/process/2026-08-03-package-anchored-subsystem-pages.md](../02-notes/implemented/process/2026-08-03-package-anchored-subsystem-pages.md)
- Pinned source: [.agents/notes/implemented/process/2026-08-03-package-anchored-subsystem-pages.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/process/2026-08-03-package-anchored-subsystem-pages.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`docs/subsystems/core.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/subsystems/core.md) | package contract and examples | The source note names this file directly. Contains the exact code literal `packages/core` named by the note. | `exact-code-occurrence, named-file` |
| [`docs/subsystems/README.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/subsystems/README.md) | package contract and examples | Entry point or contract under the directory named by the note: `docs/subsystems`. The source note names this file directly. | `exact-code-occurrence, named-directory-member, named-file` |
| [`docs/subsystems/session.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/subsystems/session.md) | package contract and examples | The source note names this file directly. | `named-file` |
| [`docs/subsystems/llm-streaming.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/subsystems/llm-streaming.md) | package contract and examples | The source note names this file directly. Contains the exact code literal `packages/llm` named by the note. | `exact-code-occurrence, named-file` |
| [`packages/core/agent/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent/src/index.ts) | package entry point | Entry point or contract under the directory named by the note: `packages/core`. Entry point or contract under the directory named by the note: `packages/core/agent`. | `named-directory-member, named-package-member, symbol-definition` |
| [`packages/core/agent/src/types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent/src/types.ts) | public types and contract | Entry point or contract under the directory named by the note: `packages/core/agent`. Core file in the package named by the note: `packages/core/agent`. | `named-directory-member, named-package-member` |
| [`packages/core/agent/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent/src/invariant.ts) | runtime contract checks | Entry point or contract under the directory named by the note: `packages/core/agent`. Core file in the package named by the note: `packages/core/agent`. | `named-directory-member, named-package-member` |
| [`packages/core/agent/src/runtime-types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent/src/runtime-types.ts) | runtime implementation | Entry point or contract under the directory named by the note: `packages/core`. Entry point or contract under the directory named by the note: `packages/core/agent`. | `named-directory-member, named-package-member` |
| [`packages/llm/README.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/README.md) | package contract and examples | Entry point or contract under the directory named by the note: `packages/llm`. | `named-directory-member` |
| [`packages/core/README.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/README.md) | package contract and examples | Entry point or contract under the directory named by the note: `packages/core`. | `named-directory-member` |
| [`packages/core/agent/README.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent/README.md) | package contract and examples | Entry point or contract under the directory named by the note: `packages/core/agent`. Core file in the package named by the note: `packages/core/agent`. | `named-directory-member, named-package-member` |
| [`packages/core/agent/package.json`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent/package.json) | composition and configuration | Entry point or contract under the directory named by the note: `packages/core/agent`. Core file in the package named by the note: `packages/core/agent`. | `named-directory-member, named-package-member` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `CreateAgentOptions` | `interface` | [`packages/core/agent/src/index.ts:80`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent/src/index.ts#L80) | `export interface CreateAgentOptions {` |
| `ResumeAgentOptions` | `interface` | [`packages/core/agent/src/index.ts:139`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent/src/index.ts#L139) | `export interface ResumeAgentOptions {` |
| `AgentHandle` | `interface` | [`packages/core/agent/src/index.ts:172`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent/src/index.ts#L172) | `export interface AgentHandle {` |
| `AgentFactory` | `interface` | [`packages/core/agent/src/index.ts:183`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent/src/index.ts#L183) | `export interface AgentFactory {` |
| `SessionEvent` | `type` | [`packages/core/session/src/types.ts:404`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/types.ts#L404) | `export type SessionEvent<T extends SessionEventType = SessionEventType> = {` |
| `TYPE_LINK_EXEMPTIONS` | `const` | [`scripts/gen-cordis-catalog.ts:507`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/gen-cordis-catalog.ts#L507) | `export const TYPE_LINK_EXEMPTIONS: Readonly<Record<string, string>> = {` |

### Tests and executable evidence

- [`scripts/project-doc-site.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/project-doc-site.spec.ts) — The source note names this file directly. Contains the exact code literal `docs/subsystems/` named by the note.
- [`packages/core/agent/tests/agent.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent/tests/agent.spec.ts) — A test under the owning area exercises or imports `CreateAgentOptions`. A test under the owning area exercises or imports `ResumeAgentOptions`.
- [`packages/typert/generator/tests/cordis-catalog.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/typert/generator/tests/cordis-catalog.spec.ts) — Contains the exact code literal `docs/subsystems/` named by the note.

## How to read the implementation

1. Start with [`docs/subsystems/core.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/subsystems/core.md) because it has the strongest evidence link to the note.
2. Read public types and entry points before implementation details. Write down the contract in plain language.
3. Follow the named constructs into their callers and tests. Confirm which component can actually enforce the rule.
4. Reconstruct one failure path: cancellation, crash, retry, stale state, partial write, or incompatible provider input—whichever the note is protecting against.
5. Only then map the contract into the current Rust repository. Target paths must come from that repository, never from guesswork in this wiki.

## Rust harness port

**Likely integration point:** repository tests and engineering policy.

Preserve the behavior and ownership rule. Do not mechanically translate Cordis, TypeScript declaration merging, Node streams, or package topology into Rust.

Relevant standing rules:

- **`rust/append-log`** — Keep durable facts append-only and derive views from them. Separate the synchronous fact boundary from asynchronous persistence.
- **`rust/four-tool-surface`** — Preserve the four model tools: read, write, patch, and exec. Reuse internal capabilities without automatically exposing new model-facing tools.
- **`rust/newtype-ids`** — Represent identity domains with Rust newtypes; do not pass bare strings across session, call, turn, step, and agent boundaries.
- **`rust/process-discipline`** — Treat this as repository or release policy unless it changes a runtime contract. Do not add runtime machinery for a process-only lesson.
- **`rust/provider-adapter`** — Keep provider wire quirks inside adapters; the loop and durable event vocabulary remain provider-neutral.
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

- Tags: `class/process`, `concern/boundary`, `concern/cancellation-timeout`, `concern/discovery-routing`, `concern/evidence`, `concern/ownership`, `concern/recovery`, `domain/build-release`, `domain/llm`, `domain/session-state`, `domain/testing`, `lifecycle/implemented`, `mechanism/policy`, `mechanism/projection`
- Aliases: `CreateAgentOptions`, `ResumeAgentOptions`, `AgentHandle`, `AgentFactory`, `SessionEvent`, `TYPE_LINK_EXEMPTIONS`, `docs/subsystems/`, `packages/core`, `packages/llm`, `…Map → derived-union`, `LINK_MAP → core.md`, `packages/<group>/README.md`, `scripts/project-doc-site.spec.ts`, `packages/core/agent`
- Regex: `(?i)(CreateAgentOptions|ResumeAgentOptions|AgentHandle|AgentFactory|SessionEvent|TYPE_LINK_EXEMPTIONS|docs/subsystems/|packages/core)`

```bash
rg -n --pcre2 "(?i)(CreateAgentOptions|ResumeAgentOptions|AgentHandle|AgentFactory|SessionEvent|TYPE_LINK_EXEMPTIONS|docs/subsystems/|packages/core)" source-deepseek-harness
rg -l --fixed-strings "class/process" 03-agent-context
```

## Connected notes

- **`source-link`** — [0383. Subsystems catalog and the `ts type-equiv` drift gate](0383-subsystems-catalog-and-the-ts-type-equiv-drift-gate.md): The source note links to this decision directly.
- **`shares-code-with`** — [0038. Initiating Agent scope over AsyncLocalStorage](0038-initiating-agent-scope-over-asynclocalstorage.md): Shares source implementation: `docs/subsystems/core.md`, `packages/core/agent/src/index.ts`.
- **`shares-code-with`** — [0031. The agent is a registration scope](0031-the-agent-is-a-registration-scope.md): Shares source implementation: `docs/subsystems/core.md`, `packages/core/agent/src/index.ts`.
- **`shares-code-with`** — [0021. Event-domain semantics --- session is the fact log, agent is the live event channel](0021-event-domain-semantics-session-is-the-fact-log-agent-is-the-live-event-c.md): Shares source implementation: `docs/subsystems/README.md`, `packages/core/agent/src/index.ts`.
- **`shares-code-with`** — [0363. Bounded background job admission](0363-bounded-background-job-admission.md): Shares source implementation: `packages/core/agent/src/index.ts`, `packages/core/agent/src/invariant.ts`.
- **`shares-code-with`** — [0283. Durable workflow runs in Chat](0283-durable-workflow-runs-in-chat.md): Shares source implementation: `packages/core/agent/src/index.ts`, `packages/core/agent/src/invariant.ts`.
- **`shares-code-with`** — [0264. Continuable subagent current-turn interrupt](0264-continuable-subagent-current-turn-interrupt.md): Shares source implementation: `packages/core/agent/src/index.ts`, `packages/core/agent/src/invariant.ts`.
- **`shares-code-with`** — [0164. Same-session goal-round driver](0164-same-session-goal-round-driver.md): Shares source implementation: `packages/core/agent/src/index.ts`, `packages/core/agent/src/invariant.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0423-package-anchored-subsystem-pages-and-thin-group-readmes.md`.
