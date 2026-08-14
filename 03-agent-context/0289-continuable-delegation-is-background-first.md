---
id: "dsh-note-0289"
title: "Continuable delegation is background-first"
status: "implemented"
class: "feature"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/feature/2026-08-11-background-first-continuable-delegation.md"
implementation_evidence: "medium"
target_anchor: "Rust provider-adapter boundary"
tags:
  - "class/feature"
  - "concern/boundary"
  - "concern/concurrency"
  - "concern/discovery-routing"
  - "concern/evidence"
  - "concern/lifecycle"
  - "concern/recovery"
  - "concern/schema-types"
  - "domain/build-release"
  - "domain/configuration"
  - "domain/llm"
  - "domain/multi-agent"
  - "domain/shell-terminal"
  - "domain/storage"
  - "domain/testing"
  - "domain/tools"
  - "lifecycle/implemented"
  - "mechanism/policy"
aliases:
  - "wakeup"
  - "report"
  - "run_in_background"
  - "tool-subagent"
  - "backgroundMode: continuable"
  - "backgroundMode: one-shot"
  - "enableRunInBackground: false"
  - "send_message"
  - "tool:<toolName>"
  - "reportDelivery"
  - "subagent-settlement"
  - "run_in_foreground"
  - "backgroundMode"
  - "run_in_background: true"
search_regex: "(?i)(wakeup|report|run_in_background|tool\\-subagent|backgroundMode:[- ]continuable|backgroundMode:[- ]one\\-shot|enableRunInBackground:[- ]false|send_message)"
---

# 0289. Continuable delegation is background-first — implementation context

## Open this when

A continuable child already has a durable id, independent turns, follow-up messaging, and a manager-owned settlement notice. Treating an omitted run_in_background as foreground makes that lifecycle depend on the model restating true on every call. It also obscures the useful scheduling test: the parent should wait only when its next action requires the child's result. The child-scoped report prompt requires a self-contained final report, while manager-owned settlement delivery independently sends the run outcome and closing message.

## Source decision

tool-subagent resolves an omitted run_in_background from the selected lifecycle policy. backgroundMode: continuable resolves omission to background and returns the durable child id immediately; explicit false selects foreground and waits for the result. backgroundMode: one-shot keeps its foreground default because background output still requires Task collection. enableRunInBackground: false continues to omit the parameter, reject forced true, and run in the foreground. No second default-selection config is added.

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/feature/2026-08-11-background-first-continuable-delegation.md](../02-notes/implemented/feature/2026-08-11-background-first-continuable-delegation.md)
- Pinned source: [.agents/notes/implemented/feature/2026-08-11-background-first-continuable-delegation.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/feature/2026-08-11-background-first-continuable-delegation.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`packages/subagent/tool-subagent/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/subagent/tool-subagent/src/index.ts) | package entry point | Core file in the package named by the note: `packages/subagent/tool-subagent`. | `named-package-member` |
| [`packages/subagent/tool-subagent/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/subagent/tool-subagent/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/subagent/tool-subagent`. | `named-package-member` |
| [`packages/subagent/tool-subagent`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/packages/subagent/tool-subagent) | package or module directory | The note names this package or capability. | `named-package` |
| [`packages/core/tools/src/code-mode.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/code-mode.ts) | runtime implementation | Defines `wakeup`, a construct named by the note. | `symbol-definition` |
| [`packages/typert/registry/src/service.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/typert/registry/src/service.ts) | runtime implementation | Defines `report`, a construct named by the note. | `symbol-definition` |
| [`packages/subagent/tool-subagent/README.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/subagent/tool-subagent/README.md) | package contract and examples | Core file in the package named by the note: `packages/subagent/tool-subagent`. | `named-package-member` |
| [`packages/subagent/tool-subagent/package.json`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/subagent/tool-subagent/package.json) | composition and configuration | Core file in the package named by the note: `packages/subagent/tool-subagent`. | `named-package-member` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `wakeup` | `const` | [`packages/core/tools/src/code-mode.ts:381`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/code-mode.ts#L381) | `const wakeup = (): void => {` |
| `report` | `const` | [`packages/typert/registry/src/service.ts:456`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/typert/registry/src/service.ts#L456) | `const report: ReportObserverError = (change, error) => {` |

### Tests and executable evidence

- [`packages/subagent/tool-subagent/tests/scripted-provider.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/subagent/tool-subagent/tests/scripted-provider.ts) — A test under the owning area exercises or imports `tool-subagent`.
- [`packages/subagent/tool-subagent/tests/tool-subagent.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/subagent/tool-subagent/tests/tool-subagent.spec.ts) — A test under the owning area exercises or imports `run_in_background`. A test under the owning area exercises or imports `tool-subagent`.

## How to read the implementation

1. Start with [`packages/subagent/tool-subagent/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/subagent/tool-subagent/src/index.ts) because it has the strongest evidence link to the note.
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
- **`rust/tokio-cancellation`** — Thread one cancellation token through async work, close owned resources before reuse, and test cancellation at every await boundary.
- **`rust/trait-seam`** — Use a small trait at the capability boundary; keep policy and model-facing schemas outside the backend implementation.

### Smallest safe implementation slice

1. Express the semantic contract as Rust types and one owning boundary.
2. Add the smallest adapter or service needed by the existing loop; do not add a new model-facing tool unless the four-tool surface cannot express the job.
3. Record durable evidence before deriving summaries or UI state.
4. Add one happy-path test, one interruption test, and one replay/restart test where state survives a turn.
5. Keep the patch reversible until the source note's strongest failure case is proven in the Rust harness.

## Search handles

- Tags: `class/feature`, `concern/boundary`, `concern/concurrency`, `concern/discovery-routing`, `concern/evidence`, `concern/lifecycle`, `concern/recovery`, `concern/schema-types`, `domain/build-release`, `domain/configuration`, `domain/llm`, `domain/multi-agent`, `domain/shell-terminal`, `domain/storage`, `domain/testing`, `domain/tools`, `lifecycle/implemented`, `mechanism/policy`
- Aliases: `wakeup`, `report`, `run_in_background`, `tool-subagent`, `backgroundMode: continuable`, `backgroundMode: one-shot`, `enableRunInBackground: false`, `send_message`, `tool:<toolName>`, `reportDelivery`, `subagent-settlement`, `run_in_foreground`, `backgroundMode`, `run_in_background: true`
- Regex: `(?i)(wakeup|report|run_in_background|tool\-subagent|backgroundMode:[- ]continuable|backgroundMode:[- ]one\-shot|enableRunInBackground:[- ]false|send_message)`

```bash
rg -n --pcre2 "(?i)(wakeup|report|run_in_background|tool\\-subagent|backgroundMode:[- ]continuable|backgroundMode:[- ]one\\-shot|enableRunInBackground:[- ]false|send_message)" source-deepseek-harness
rg -l --fixed-strings "class/feature" 03-agent-context
```

## Connected notes

- **`source-link`** — [0263. The continuable child return channel is an obligation](0263-the-continuable-child-return-channel-is-an-obligation.md): The source note links to this decision directly.
- **`source-link`** — [0265. Settlement delivery belongs to the continuation manager](0265-settlement-delivery-belongs-to-the-continuation-manager.md): The source note links to this decision directly.
- **`shares-code-with`** — [0664. Retire the standalone subagent mock package](0664-retire-the-standalone-subagent-mock-package.md): Shares source implementation: `packages/subagent/tool-subagent`, `packages/subagent/tool-subagent/README.md`.
- **`shares-code-with`** — [0662. Drop unconsumed skill provider events](0662-drop-unconsumed-skill-provider-events.md): Shares source implementation: `packages/subagent/tool-subagent`, `packages/subagent/tool-subagent/README.md`.
- **`shares-code-with`** — [0120. Product subagent providers live in the shared profile host](0120-product-subagent-providers-live-in-the-shared-profile-host.md): Shares source implementation: `packages/subagent/tool-subagent`, `packages/subagent/tool-subagent/README.md`.
- **`shares-code-with`** — [0148. Background subagent tasks](0148-background-subagent-tasks.md): Shares source implementation: `packages/subagent/tool-subagent`, `packages/subagent/tool-subagent/src/index.ts`.
- **`shares-code-with`** — [0203. SDK max output tokens](0203-sdk-max-output-tokens.md): Shares source implementation: `packages/subagent/tool-subagent`, `packages/subagent/tool-subagent/src/index.ts`.
- **`shares-code-with`** — [0153. Configure subagent persona, tool visibility, and depth](0153-configure-subagent-persona-tool-visibility-and-depth.md): Shares source implementation: `packages/subagent/tool-subagent`, `packages/subagent/tool-subagent/src/index.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0289-continuable-delegation-is-background-first.md`.
