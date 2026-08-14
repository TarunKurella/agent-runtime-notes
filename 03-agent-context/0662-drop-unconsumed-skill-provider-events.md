---
id: "dsh-note-0662"
title: "Drop unconsumed skill provider events"
status: "archived"
class: "simplification"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/archived/simplification/2026-07-12-drop-unconsumed-skill-provider-events.md"
implementation_evidence: "medium"
target_anchor: "Rust provider-adapter boundary"
tags:
  - "class/simplification"
  - "concern/boundary"
  - "concern/concurrency"
  - "concern/discovery-routing"
  - "concern/evidence"
  - "concern/lifecycle"
  - "concern/performance"
  - "domain/build-release"
  - "domain/configuration"
  - "domain/extensions"
  - "domain/llm"
  - "domain/multi-agent"
  - "domain/observability"
  - "lifecycle/archived"
  - "mechanism/generation"
  - "mechanism/registry"
aliases:
  - "removed"
  - "skill/provider-added"
  - "skill/provider-removed"
  - "subagent/provider-added"
  - "tools/change"
  - "system-prompt/change"
  - "tool-subagent"
  - "Drop unconsumed skill provider events"
  - "simplification"
  - "boundary"
  - "concurrency"
  - "discovery routing"
  - "evidence"
  - "lifecycle"
search_regex: "(?i)(removed|skill/provider\\-added|skill/provider\\-removed|subagent/provider\\-added|tools/change|system\\-prompt/change|tool\\-subagent|Drop[- ]unconsumed[- ]skill[- ]provider[- ]events)"
---

# 0662. Drop unconsumed skill provider events — implementation context

## Open this when

Two skill-registry notifications are produced but have no production listener. The generated producer/consumer matrix and exact event-name searches find only declarations, emit sites, tests, generated catalogs, and prose for skill/provider-added and skill/provider-removed. Skill discovery reads the current provider map on demand, provider registration synchronously clears completed catalogs, and the post-await revision check prevents stale discovery from entering the cache.

## Source decision

The skill registry declares and emits no provider-membership events. Provider registration and disposal remain direct effect-owned state changes that synchronously invalidate completed catalogs; lookup and discovery read the current provider map on demand. Tests observe cleanup through provider lookup and collected output rather than lifecycle notifications. The generated event catalog, API catalog, and producer/consumer matrix omit the deleted notifications. The skill-system Agent Note and package documentation describe registration through its direct effect-owned state and cache-invalidation contract.

## Decision status

Historical design. Use it to understand an older pressure or failure; locate the replacing decision before implementing anything.

- Raw note: [archived/simplification/2026-07-12-drop-unconsumed-skill-provider-events.md](../02-notes/archived/simplification/2026-07-12-drop-unconsumed-skill-provider-events.md)
- Pinned source: [.agents/notes/archived/simplification/2026-07-12-drop-unconsumed-skill-provider-events.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/archived/simplification/2026-07-12-drop-unconsumed-skill-provider-events.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`packages/subagent/tool-subagent/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/subagent/tool-subagent/src/index.ts) | package entry point | Core file in the package named by the note: `packages/subagent/tool-subagent`. Contains the exact code literal `subagent/provider-added` named by the note. | `exact-code-occurrence, named-package-member` |
| [`packages/subagent/tool-subagent/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/subagent/tool-subagent/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/subagent/tool-subagent`. | `named-package-member` |
| [`packages/subagent/tool-subagent`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/packages/subagent/tool-subagent) | package or module directory | The note names this package or capability. | `named-package` |
| [`packages/core/agent/src/inbox.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent/src/inbox.ts) | runtime implementation | Defines `removed`, a construct named by the note. | `symbol-definition` |
| [`packages/subagent/tool-subagent/README.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/subagent/tool-subagent/README.md) | package contract and examples | Core file in the package named by the note: `packages/subagent/tool-subagent`. | `named-package-member` |
| [`packages/subagent/tool-subagent/package.json`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/subagent/tool-subagent/package.json) | composition and configuration | Core file in the package named by the note: `packages/subagent/tool-subagent`. | `named-package-member` |
| [`docs/subsystems/tools.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/subsystems/tools.md) | package contract and examples | Contains the exact code literal `tools/change` named by the note. | `exact-code-occurrence` |
| [`docs/subsystems/subagent.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/subsystems/subagent.md) | package contract and examples | Contains the exact code literal `subagent/provider-added` named by the note. | `exact-code-occurrence` |
| [`docs/subsystems/tools.zh.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/subsystems/tools.zh.md) | package contract and examples | Contains the exact code literal `tools/change` named by the note. | `exact-code-occurrence` |
| [`docs/subsystems/subagent.zh.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/subsystems/subagent.zh.md) | package contract and examples | Contains the exact code literal `subagent/provider-added` named by the note. | `exact-code-occurrence` |
| [`docs/event-producer-consumer.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/event-producer-consumer.md) | package contract and examples | Contains the exact code literal `subagent/provider-added` named by the note. Contains the exact code literal `tools/change` named by the note. | `exact-code-occurrence` |
| [`packages/core/tools/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/index.ts) | package entry point | Contains the exact code literal `tools/change` named by the note. | `exact-code-occurrence` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `removed` | `const` | [`packages/core/agent/src/inbox.ts:187`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent/src/inbox.ts#L187) | `const removed = inbox.splice(actualStart, actualDeleteCount, ...event.data.inserted)` |

### Tests and executable evidence

- [`packages/subagent/tool-subagent/tests/scripted-provider.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/subagent/tool-subagent/tests/scripted-provider.ts) — A test under the owning area exercises or imports `tool-subagent`.
- [`packages/subagent/tool-subagent/tests/tool-subagent.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/subagent/tool-subagent/tests/tool-subagent.spec.ts) — A test under the owning area exercises or imports `tool-subagent`.
- [`packages/core/tools/tests/tools.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/tests/tools.spec.ts) — Contains the exact code literal `tools/change` named by the note.
- [`packages/core/tools/tests/invariant.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/tests/invariant.spec.ts) — Contains the exact code literal `tools/change` named by the note.
- [`packages/typert/registry/tests/typert.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/typert/registry/tests/typert.spec.ts) — Contains the exact code literal `tools/change` named by the note.
- [`packages/core/system-prompt/tests/system-prompt.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/system-prompt/tests/system-prompt.spec.ts) — Contains the exact code literal `system-prompt/change` named by the note.
- [`packages/subagent/tool-subagent-report/tests/tool-subagent-report.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/subagent/tool-subagent-report/tests/tool-subagent-report.spec.ts) — Contains the exact code literal `system-prompt/change` named by the note.

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

- **`rust/four-tool-surface`** — Preserve the four model tools: read, write, patch, and exec. Reuse internal capabilities without automatically exposing new model-facing tools.
- **`rust/process-discipline`** — Treat this as repository or release policy unless it changes a runtime contract. Do not add runtime machinery for a process-only lesson.
- **`rust/provider-adapter`** — Keep provider wire quirks inside adapters; the loop and durable event vocabulary remain provider-neutral.
- **`rust/quickjs-boundary`** — QuickJS holds orchestration variables; Rust/Tokio owns async execution and exposes only explicitly granted capabilities.
- **`rust/trait-seam`** — Use a small trait at the capability boundary; keep policy and model-facing schemas outside the backend implementation.
- **Status gate** — Historical design. Use it to understand an older pressure or failure; locate the replacing decision before implementing anything.

### Smallest safe implementation slice

1. Express the semantic contract as Rust types and one owning boundary.
2. Add the smallest adapter or service needed by the existing loop; do not add a new model-facing tool unless the four-tool surface cannot express the job.
3. Record durable evidence before deriving summaries or UI state.
4. Add one happy-path test, one interruption test, and one replay/restart test where state survives a turn.
5. Keep the patch reversible until the source note's strongest failure case is proven in the Rust harness.

## Search handles

- Tags: `class/simplification`, `concern/boundary`, `concern/concurrency`, `concern/discovery-routing`, `concern/evidence`, `concern/lifecycle`, `concern/performance`, `domain/build-release`, `domain/configuration`, `domain/extensions`, `domain/llm`, `domain/multi-agent`, `domain/observability`, `lifecycle/archived`, `mechanism/generation`, `mechanism/registry`
- Aliases: `removed`, `skill/provider-added`, `skill/provider-removed`, `subagent/provider-added`, `tools/change`, `system-prompt/change`, `tool-subagent`, `Drop unconsumed skill provider events`, `simplification`, `boundary`, `concurrency`, `discovery routing`, `evidence`, `lifecycle`
- Regex: `(?i)(removed|skill/provider\-added|skill/provider\-removed|subagent/provider\-added|tools/change|system\-prompt/change|tool\-subagent|Drop[- ]unconsumed[- ]skill[- ]provider[- ]events)`

```bash
rg -n --pcre2 "(?i)(removed|skill/provider\\-added|skill/provider\\-removed|subagent/provider\\-added|tools/change|system\\-prompt/change|tool\\-subagent|Drop[- ]unconsumed[- ]skill[- ]provider[- ]events)" source-deepseek-harness
rg -l --fixed-strings "class/simplification" 03-agent-context
```

## Connected notes

- **`shares-code-with`** — [0664. Retire the standalone subagent mock package](0664-retire-the-standalone-subagent-mock-package.md): Shares source implementation: `packages/subagent/tool-subagent`, `packages/subagent/tool-subagent/README.md`.
- **`shares-code-with`** — [0289. Continuable delegation is background-first](0289-continuable-delegation-is-background-first.md): Shares source implementation: `packages/subagent/tool-subagent`, `packages/subagent/tool-subagent/README.md`.
- **`shares-code-with`** — [0120. Product subagent providers live in the shared profile host](0120-product-subagent-providers-live-in-the-shared-profile-host.md): Shares source implementation: `packages/subagent/tool-subagent`, `packages/subagent/tool-subagent/README.md`.
- **`shares-code-with`** — [0026. Subagent provider-lifecycle events --- `subagent/provider-added` / `subagent/provider-removed`](0026-subagent-provider-lifecycle-events-subagent-provider-added-subagent-prov.md): Shares source implementation: `docs/subsystems/subagent.md`, `packages/subagent/tool-subagent`.
- **`shares-code-with`** — [0203. SDK max output tokens](0203-sdk-max-output-tokens.md): Shares source implementation: `packages/subagent/tool-subagent`, `packages/subagent/tool-subagent/src/index.ts`.
- **`shares-code-with`** — [0153. Configure subagent persona, tool visibility, and depth](0153-configure-subagent-persona-tool-visibility-and-depth.md): Shares source implementation: `packages/subagent/tool-subagent`, `packages/subagent/tool-subagent/src/index.ts`.
- **`shares-code-with`** — [0462. Merge subagent control into the subagent service](0462-merge-subagent-control-into-the-subagent-service.md): Shares source implementation: `packages/subagent/tool-subagent`, `packages/subagent/tool-subagent/src/index.ts`.
- **`shares-code-with`** — [0148. Background subagent tasks](0148-background-subagent-tasks.md): Shares source implementation: `packages/subagent/tool-subagent`, `packages/subagent/tool-subagent/src/index.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0662-drop-unconsumed-skill-provider-events.md`.
