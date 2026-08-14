---
id: "dsh-note-0117"
title: "Forked children stay one-shot"
status: "implemented"
class: "architecture"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/architecture/2026-08-10-fork-children-stay-one-shot.md"
implementation_evidence: "high"
target_anchor: "Rust provider-adapter boundary"
tags:
  - "class/architecture"
  - "concern/boundary"
  - "concern/compatibility"
  - "concern/discovery-routing"
  - "concern/evidence"
  - "concern/lifecycle"
  - "concern/ownership"
  - "concern/schema-types"
  - "domain/build-release"
  - "domain/extensions"
  - "domain/llm"
  - "domain/multi-agent"
  - "domain/protocols"
  - "domain/session-state"
  - "domain/storage"
  - "domain/tools"
  - "lifecycle/implemented"
  - "mechanism/adapter"
  - "mechanism/policy"
  - "mechanism/registry"
aliases:
  - "wakeup"
  - "persona"
  - "toolFilter"
  - "continuable"
  - "report"
  - "tasks"
  - "backgroundMode: one-shot"
  - "run_in_background"
  - "enableRunInBackground: false"
  - "SubagentRuntime.start"
  - "backgroundMode: continuable"
  - "ForkInProcessProvider.prepareContinuable"
  - "prepareContinuable"
  - "ctx.subagents.startContinuable"
search_regex: "(?i)(wakeup|persona|toolFilter|continuable|report|tasks|backgroundMode:[- ]one\\-shot|run_in_background)"
---

# 0117. Forked children stay one-shot — implementation context

## Open this when

Fork's only difference from spawn is that the child Session is seeded with the parent's completed-turn prefix (subagent-fork-in-process). That seed costs real tokens --- the inherited history is re-sent in every child request --- and its one concrete payoff is provider-side prefix reuse: under the same provider and model, a child request whose leading bytes are identical to the parent's re-prefills none of the shared span. Anything a child scope adds ahead of the inherited history spends that payoff, because reuse stops at the first differing byte.

## Source decision

Every shipped composition binds the fork delegation tool to backgroundMode: one-shot: the base bundle, the ACP example, and the headless example. The base bundle leaves run_in_background available, because it mounts a task service; the two examples set enableRunInBackground: false, because they mount none and a one-shot background start would otherwise fail at call time on a missing tasks service.

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/architecture/2026-08-10-fork-children-stay-one-shot.md](../02-notes/implemented/architecture/2026-08-10-fork-children-stay-one-shot.md)
- Pinned source: [.agents/notes/implemented/architecture/2026-08-10-fork-children-stay-one-shot.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/architecture/2026-08-10-fork-children-stay-one-shot.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`examples/acp-agent/cordis.yml`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/examples/acp-agent/cordis.yml) | composition and configuration | The source note names this file directly. | `named-file` |
| [`examples/headless-agent/cordis.yml`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/examples/headless-agent/cordis.yml) | composition and configuration | The source note names this file directly. | `named-file` |
| [`packages/bundle/base/cordis.patch.yml`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/bundle/base/cordis.patch.yml) | composition and configuration | The source note names this file directly. | `named-file` |
| [`packages/subagent/tool-subagent-report/README.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/subagent/tool-subagent-report/README.md) | package contract and examples | The source note names this file directly. Core file in the package named by the note: `packages/subagent/tool-subagent-report`. | `named-file, named-package-member` |
| [`packages/subagent/subagent-fork-in-process/README.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/subagent/subagent-fork-in-process/README.md) | package contract and examples | The source note names this file directly. | `named-file` |
| [`packages/preset/persona/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/preset/persona/src/index.ts) | package entry point | Core file in the package named by the note: `packages/preset/persona`. | `named-package-member` |
| [`packages/preset/persona/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/preset/persona/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/preset/persona`. | `named-package-member` |
| [`packages/subagent/tool-subagent/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/subagent/tool-subagent/src/index.ts) | package entry point | Core file in the package named by the note: `packages/subagent/tool-subagent`. Defines `continuable`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/subagent/tool-subagent/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/subagent/tool-subagent/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/subagent/tool-subagent`. | `named-package-member` |
| [`packages/subagent/tool-subagent-report/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/subagent/tool-subagent-report/src/index.ts) | package entry point | Core file in the package named by the note: `packages/subagent/tool-subagent-report`. | `named-package-member` |
| [`packages/subagent/tool-subagent-report/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/subagent/tool-subagent-report/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/subagent/tool-subagent-report`. | `named-package-member` |
| [`packages/preset/persona`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/packages/preset/persona) | package or module directory | The note names this package or capability. | `named-package` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `wakeup` | `const` | [`packages/core/tools/src/code-mode.ts:381`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/code-mode.ts#L381) | `const wakeup = (): void => {` |
| `persona` | `const` | [`packages/subagent/subagent/src/descriptor.ts:234`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/subagent/subagent/src/descriptor.ts#L234) | `const persona = optionalString(value, 'persona')` |
| `toolFilter` | `const` | [`packages/subagent/subagent/src/descriptor.ts:235`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/subagent/subagent/src/descriptor.ts#L235) | `const toolFilter = Object.hasOwn(value, 'toolFilter')` |
| `continuable` | `const` | [`packages/subagent/tool-subagent/src/index.ts:276`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/subagent/tool-subagent/src/index.ts#L276) | `const continuable = (config.backgroundMode ?? 'one-shot') === 'continuable'` |
| `report` | `const` | [`packages/typert/registry/src/service.ts:456`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/typert/registry/src/service.ts#L456) | `const report: ReportObserverError = (change, error) => {` |
| `tasks` | `const` | [`vendor/loader/src/config/tree.ts:48`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/loader/src/config/tree.ts#L48) | `const tasks = this.getTasks()` |

### Tests and executable evidence

- [`packages/preset/persona/tests/persona.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/preset/persona/tests/persona.spec.ts) — A test under the owning area exercises or imports `persona`.
- [`packages/subagent/tool-subagent/tests/scripted-provider.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/subagent/tool-subagent/tests/scripted-provider.ts) — A test under the owning area exercises or imports `toolFilter`. A test under the owning area exercises or imports `tool-subagent`.
- [`packages/subagent/tool-subagent/tests/tool-subagent.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/subagent/tool-subagent/tests/tool-subagent.spec.ts) — A test under the owning area exercises or imports `run_in_background`. A test under the owning area exercises or imports `toolFilter`.
- [`packages/subagent/tool-subagent-report/tests/tool-subagent-report.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/subagent/tool-subagent-report/tests/tool-subagent-report.spec.ts) — A test under the owning area exercises or imports `toolFilter`. A test under the owning area exercises or imports `dsh-tool-subagent-report`.

## How to read the implementation

1. Start with [`examples/acp-agent/cordis.yml`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/examples/acp-agent/cordis.yml) because it has the strongest evidence link to the note.
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
- **`rust/process-discipline`** — Treat this as repository or release policy unless it changes a runtime contract. Do not add runtime machinery for a process-only lesson.
- **`rust/provider-adapter`** — Keep provider wire quirks inside adapters; the loop and durable event vocabulary remain provider-neutral.
- **`rust/quickjs-boundary`** — QuickJS holds orchestration variables; Rust/Tokio owns async execution and exposes only explicitly granted capabilities.
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

- Tags: `class/architecture`, `concern/boundary`, `concern/compatibility`, `concern/discovery-routing`, `concern/evidence`, `concern/lifecycle`, `concern/ownership`, `concern/schema-types`, `domain/build-release`, `domain/extensions`, `domain/llm`, `domain/multi-agent`, `domain/protocols`, `domain/session-state`, `domain/storage`, `domain/tools`, `lifecycle/implemented`, `mechanism/adapter`, `mechanism/policy`, `mechanism/registry`
- Aliases: `wakeup`, `persona`, `toolFilter`, `continuable`, `report`, `tasks`, `backgroundMode: one-shot`, `run_in_background`, `enableRunInBackground: false`, `SubagentRuntime.start`, `backgroundMode: continuable`, `ForkInProcessProvider.prepareContinuable`, `prepareContinuable`, `ctx.subagents.startContinuable`
- Regex: `(?i)(wakeup|persona|toolFilter|continuable|report|tasks|backgroundMode:[- ]one\-shot|run_in_background)`

```bash
rg -n --pcre2 "(?i)(wakeup|persona|toolFilter|continuable|report|tasks|backgroundMode:[- ]one\\-shot|run_in_background)" source-deepseek-harness
rg -l --fixed-strings "class/architecture" 03-agent-context
```

## Connected notes

- **`source-link`** — [0211. Continuable subagent report tool](0211-continuable-subagent-report-tool.md): The source note links to this decision directly.
- **`source-link`** — [0263. The continuable child return channel is an obligation](0263-the-continuable-child-return-channel-is-an-obligation.md): The source note links to this decision directly.
- **`shares-code-with`** — [0153. Configure subagent persona, tool visibility, and depth](0153-configure-subagent-persona-tool-visibility-and-depth.md): Shares source implementation: `packages/preset/persona/src/index.ts`, `packages/preset/persona/src/invariant.ts`.
- **`shares-code-with`** — [0249. Claude Code and Codex subagent backends](0249-claude-code-and-codex-subagent-backends.md): Shares source implementation: `packages/subagent/tool-subagent/src/index.ts`, `packages/subagent/tool-subagent/src/invariant.ts`.
- **`shares-code-with`** — [0148. Background subagent tasks](0148-background-subagent-tasks.md): Shares source implementation: `packages/subagent/tool-subagent/src/index.ts`, `packages/subagent/tool-subagent/src/invariant.ts`.
- **`shares-code-with`** — [0120. Product subagent providers live in the shared profile host](0120-product-subagent-providers-live-in-the-shared-profile-host.md): Shares source implementation: `packages/subagent/tool-subagent/src/index.ts`, `packages/subagent/tool-subagent/src/invariant.ts`.
- **`shares-code-with`** — [0203. SDK max output tokens](0203-sdk-max-output-tokens.md): Shares source implementation: `packages/subagent/tool-subagent/src/index.ts`, `packages/subagent/tool-subagent/src/invariant.ts`.
- **`shares-code-with`** — [0462. Merge subagent control into the subagent service](0462-merge-subagent-control-into-the-subagent-service.md): Shares source implementation: `packages/subagent/tool-subagent/src/index.ts`, `packages/subagent/tool-subagent/src/invariant.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0117-forked-children-stay-one-shot.md`.
