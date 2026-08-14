---
id: "dsh-note-0276"
title: "Per-Model Reasoning Declarations in llm-pi-ai"
status: "implemented"
class: "feature"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/feature/2026-08-08-pi-ai-per-model-reasoning-declarations.md"
implementation_evidence: "medium"
target_anchor: "Rust provider-adapter boundary"
tags:
  - "class/feature"
  - "concern/boundary"
  - "concern/discovery-routing"
  - "concern/evidence"
  - "concern/human-control"
  - "concern/ownership"
  - "concern/schema-types"
  - "domain/build-release"
  - "domain/configuration"
  - "domain/llm"
  - "domain/protocols"
  - "lifecycle/implemented"
aliases:
  - "off"
  - "models"
  - "PiAiModelProfile"
  - "thinkingFormat"
  - "supportsReasoningEffort"
  - "reasoningEfforts"
  - "yaml"
  - "getSupportedThinkingLevels"
  - "UNSUPPORTED_REASONING_EFFORT"
  - "settings.yaml"
  - "compat.thinkingFormat"
  - "compat.supportsReasoningEffort"
  - "gpt-5"
  - "Model.reasoning"
search_regex: "(?i)(models|PiAiModelProfile|thinkingFormat|supportsReasoningEffort|reasoningEfforts|yaml|getSupportedThinkingLevels|UNSUPPORTED_REASONING_EFFORT)"
---

# 0276. Per-Model Reasoning Declarations in llm-pi-ai — implementation context

## Open this when

Under the declared-provider catalog ([[2026-08-03-pi-ai-declared-provider-catalog]], which deliberately kept reasoning out of the configurable fields), a hand-declared pi-ai route's models materialized with reasoning: false, so getSupportedThinkingLevels short-circuited to ["off"]: the composer offered no effort picker for them, and the route-level reasoning default --- the only reasoning knob a profile had --- made every request to such a model fail with UNSUPPORTED_REASONING_EFFORT before network I/O.

## Source decision

PiAiModelProfile gains reasoningEfforts: each key is a level selectors offer, its value the spelling dispatch sends on the wire. The declaration translates to pi-ai's Model.reasoning + thinkingLevelMap with all seven levels decided explicitly --- declared levels carry their wire value, undeclared levels are pinned null --- so the profile author never needs pi-ai's asymmetric defaulting rule (absent means "supported" for the five base levels but "unsupported" for xhigh/max).

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/feature/2026-08-08-pi-ai-per-model-reasoning-declarations.md](../02-notes/implemented/feature/2026-08-08-pi-ai-per-model-reasoning-declarations.md)
- Pinned source: [.agents/notes/implemented/feature/2026-08-08-pi-ai-per-model-reasoning-declarations.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/feature/2026-08-08-pi-ai-per-model-reasoning-declarations.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`packages/llm/llm/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/src/index.ts) | package entry point | Defines `models`, a construct named by the note. | `symbol-definition` |
| [`packages/llm/llm-pi-ai/src/config.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm-pi-ai/src/config.ts) | runtime implementation | Defines `reasoningEfforts`, a construct named by the note. | `symbol-definition` |
| [`packages/llm/llm-pi-ai/src/catalog.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm-pi-ai/src/catalog.ts) | runtime implementation | Defines `thinkingFormat`, a construct named by the note. Defines `supportsReasoningEffort`, a construct named by the note. | `symbol-definition` |
| [`packages/llm/llm-pi-ai/src/adapter.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm-pi-ai/src/adapter.ts) | provider/backend adapter | Defines `models`, a construct named by the note. | `symbol-definition` |
| [`packages/skill/skill-filesystem/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/skill/skill-filesystem/src/index.ts) | package entry point | Defines `yaml`, a construct named by the note. | `symbol-definition` |
| [`packages/client/ui-layout/src/client/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-layout/src/client/index.ts) | package entry point | Defines `off`, a construct named by the note. | `symbol-definition` |
| [`packages/client/ui-plan/src/client/PlanModeControl.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-plan/src/client/PlanModeControl.tsx) | runtime implementation | Defines `off`, a construct named by the note. | `symbol-definition` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `off` | `const` | [`packages/client/ui-layout/src/client/index.ts:150`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-layout/src/client/index.ts#L150) | `const off = ctx.on('theme/change', (snapshot) => { presenter.apply(snapshot) })` |
| `off` | `const` | [`packages/client/ui-plan/src/client/PlanModeControl.tsx:36`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-plan/src/client/PlanModeControl.tsx#L36) | `const off = (): void => {` |
| `models` | `const` | [`packages/llm/llm-pi-ai/src/adapter.ts:202`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm-pi-ai/src/adapter.ts#L202) | `const models: MutableModels = createModels()` |
| `models` | `const` | [`packages/llm/llm-pi-ai/src/catalog.ts:171`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm-pi-ai/src/catalog.ts#L171) | `const models = getBuiltinModels(provider as BuiltinProvider) as Model<Api>[]` |
| `PiAiModelProfile` | `interface` | [`packages/llm/llm-pi-ai/src/catalog.ts:202`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm-pi-ai/src/catalog.ts#L202) | `export interface PiAiModelProfile {` |
| `thinkingFormat` | `const` | [`packages/llm/llm-pi-ai/src/catalog.ts:395`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm-pi-ai/src/catalog.ts#L395) | `const thinkingFormat = entry.compat?.thinkingFormat ?? route?.thinkingFormat` |
| `supportsReasoningEffort` | `const` | [`packages/llm/llm-pi-ai/src/catalog.ts:396`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm-pi-ai/src/catalog.ts#L396) | `const supportsReasoningEffort = entry.compat?.supportsReasoningEffort ?? route?.supportsReasoningEffort` |
| `models` | `const` | [`packages/llm/llm-pi-ai/src/catalog.ts:492`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm-pi-ai/src/catalog.ts#L492) | `const models = entries.map((entry) => {` |
| `reasoningEfforts` | `const` | [`packages/llm/llm-pi-ai/src/config.ts:203`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm-pi-ai/src/config.ts#L203) | `const reasoningEfforts = z.dict(` |
| `models` | `const` | [`packages/llm/llm/src/index.ts:547`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/src/index.ts#L547) | `const models: LlmDiscoveredModel[] = []` |
| `models` | `const` | [`packages/llm/llm/src/index.ts:583`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/src/index.ts#L583) | `const models = await adapter.listModels(provider)` |
| `yaml` | `const` | [`packages/skill/skill-filesystem/src/index.ts:917`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/skill/skill-filesystem/src/index.ts#L917) | `const yaml = raw.slice(start, closing.start)` |

### Tests and executable evidence

- [`scripts/run-gates.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/run-gates.spec.ts) — A test under the owning area exercises or imports `max`.
- [`scripts/test-invariants.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/test-invariants.ts) — A test under the owning area exercises or imports `Record`.
- [`apps/web/tests/scaffold.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/scaffold.ts) — A test under the owning area exercises or imports `resolveModelInfo`.
- [`scripts/ci-workflow.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/ci-workflow.spec.ts) — A test under the owning area exercises or imports `Record`.
- [`scripts/change-scope.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/change-scope.spec.ts) — A test under the owning area exercises or imports `Record`.
- [`scripts/test-invariants.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/test-invariants.spec.ts) — A test under the owning area exercises or imports `Record`.
- [`scripts/oxlint-contract.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/oxlint-contract.spec.ts) — A test under the owning area exercises or imports `Record`.
- [`apps/web/tests/default-model.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/default-model.e2e.ts) — A test under the owning area exercises or imports `openai-completions`.

## How to read the implementation

1. Start with [`packages/llm/llm/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/src/index.ts) because it has the strongest evidence link to the note.
2. Read public types and entry points before implementation details. Write down the contract in plain language.
3. Follow the named constructs into their callers and tests. Confirm which component can actually enforce the rule.
4. Reconstruct one failure path: cancellation, crash, retry, stale state, partial write, or incompatible provider input—whichever the note is protecting against.
5. Only then map the contract into the current Rust repository. Target paths must come from that repository, never from guesswork in this wiki.

## Rust harness port

**Likely integration point:** Rust provider-adapter boundary.

Preserve the behavior and ownership rule. Do not mechanically translate Cordis, TypeScript declaration merging, Node streams, or package topology into Rust.

Relevant standing rules:

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

- Tags: `class/feature`, `concern/boundary`, `concern/discovery-routing`, `concern/evidence`, `concern/human-control`, `concern/ownership`, `concern/schema-types`, `domain/build-release`, `domain/configuration`, `domain/llm`, `domain/protocols`, `lifecycle/implemented`
- Aliases: `off`, `models`, `PiAiModelProfile`, `thinkingFormat`, `supportsReasoningEffort`, `reasoningEfforts`, `yaml`, `getSupportedThinkingLevels`, `UNSUPPORTED_REASONING_EFFORT`, `settings.yaml`, `compat.thinkingFormat`, `compat.supportsReasoningEffort`, `gpt-5`, `Model.reasoning`
- Regex: `(?i)(models|PiAiModelProfile|thinkingFormat|supportsReasoningEffort|reasoningEfforts|yaml|getSupportedThinkingLevels|UNSUPPORTED_REASONING_EFFORT)`

```bash
rg -n --pcre2 "(?i)(models|PiAiModelProfile|thinkingFormat|supportsReasoningEffort|reasoningEfforts|yaml|getSupportedThinkingLevels|UNSUPPORTED_REASONING_EFFORT)" source-deepseek-harness
rg -l --fixed-strings "class/feature" 03-agent-context
```

## Connected notes

- **`shares-code-with`** — [0077. request-level LLM configuration and the credential seam](0077-request-level-llm-configuration-and-the-credential-seam.md): Shares source implementation: `packages/llm/llm-pi-ai/src/adapter.ts`, `packages/llm/llm-pi-ai/src/catalog.ts`.
- **`shares-code-with`** — [0376. The configurable-provider directory withholds OAuth-only providers](0376-the-configurable-provider-directory-withholds-oauth-only-providers.md): Shares source implementation: `packages/llm/llm-pi-ai/src/adapter.ts`, `packages/llm/llm-pi-ai/src/catalog.ts`.
- **`shares-code-with`** — [0128. A pi-ai model declares its own input modalities, and undeclared means text](0128-a-pi-ai-model-declares-its-own-input-modalities-and-undeclared-means-tex.md): Shares source implementation: `packages/llm/llm-pi-ai/src/adapter.ts`, `packages/llm/llm-pi-ai/src/catalog.ts`.
- **`shares-code-with`** — [0433. Standardize Chinese contract terminology on 约定](0433-standardize-chinese-contract-terminology-on-contract.md): Shares source implementation: `apps/web/tests/scaffold.ts`, `scripts/run-gates.spec.ts`.
- **`shares-code-with`** — [0008. Two LLM adapters as a design-verification twin](0008-two-llm-adapters-as-a-design-verification-twin.md): Shares source implementation: `packages/llm/llm-pi-ai/src/adapter.ts`, `packages/llm/llm-pi-ai/src/catalog.ts`.
- **`shares-code-with`** — [0500. Keyless browser e2e lane for the web GUI](0500-keyless-browser-e2e-lane-for-the-web-gui.md): Shares source implementation: `apps/web/tests/scaffold.ts`, `packages/llm/llm/src/index.ts`.
- **`shares-code-with`** — [0098. Interrogating a draft provider endpoint](0098-interrogating-a-draft-provider-endpoint.md): Shares source implementation: `packages/llm/llm-pi-ai/src/adapter.ts`, `packages/llm/llm-pi-ai/src/catalog.ts`.
- **`shares-code-with`** — [0097. Declaring a provider from the Models page](0097-declaring-a-provider-from-the-models-page.md): Shares source implementation: `packages/llm/llm-pi-ai/src/adapter.ts`, `packages/llm/llm-pi-ai/src/catalog.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0276-per-model-reasoning-declarations-in-llm-pi-ai.md`.
