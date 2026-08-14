---
id: "dsh-note-0458"
title: "Plan-specific collaboration state"
status: "implemented"
class: "simplification"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/simplification/2026-07-22-plan-specific-collaboration-state.md"
implementation_evidence: "medium"
target_anchor: "sandbox capability boundary"
tags:
  - "class/simplification"
  - "concern/boundary"
  - "concern/cancellation-timeout"
  - "concern/compatibility"
  - "concern/discovery-routing"
  - "concern/evidence"
  - "concern/human-control"
  - "concern/lifecycle"
  - "concern/ownership"
  - "concern/recovery"
  - "concern/schema-types"
  - "concern/trust"
  - "domain/agent-loop"
  - "domain/build-release"
  - "domain/configuration"
  - "domain/context"
  - "domain/extensions"
  - "domain/filesystem"
  - "domain/llm"
  - "domain/protocols"
  - "domain/security"
  - "domain/session-state"
  - "domain/storage"
  - "domain/testing"
  - "domain/tools"
  - "domain/ui-interaction"
  - "lifecycle/implemented"
  - "mechanism/adapter"
  - "mechanism/policy"
  - "mechanism/projection"
  - "mechanism/registry"
aliases:
  - "modes"
  - "review"
  - "plan"
  - "sandboxPolicy"
  - "section"
  - "foldPlanMode"
  - "set"
  - "ModeConfig.modes"
  - "ctx.modes.list"
  - "/plan"
  - "exit_plan_mode"
  - "ctx.sandboxPolicy"
  - "sandbox/mode"
  - "@deepseek-ai/dsh-plan-mode"
search_regex: "(?i)(modes|review|plan|sandboxPolicy|section|foldPlanMode|ModeConfig\\.modes|ctx\\.modes\\.list)"
---

# 0458. Plan-specific collaboration state — implementation context

## Open this when

The first plan-mode implementation introduced a generic named-mode registry even though the product shipped only plan. ModeConfig.modes, definition-name validation, ctx.modes.list(), retired-definition fallback, and a synthetic review mode in tests existed only to support hypothetical future collaboration modes. The production-specific behavior---plan guidance, /plan, and exit_plan_mode---still lived in the same package, so the generic API did not isolate a reusable mechanism from plan policy. The word "mode" also spans unrelated domains.

## Source decision

Plan mode owns a plan-specific product package: @deepseek-ai/dsh-plan-mode at packages/plan/plan-mode/. The durable fact is plan/mode: { active: boolean }, folded by foldPlanMode(events) with false as the empty-log value. ctx.planMode.get(agent) returns { active, pending? }, and set(agent, active) records the boundary-applied selection. The pre-step, retry, append-failure, and disposal fences preserve the same state-transition ownership. Configuration is exactly { section: string }.

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/simplification/2026-07-22-plan-specific-collaboration-state.md](../02-notes/implemented/simplification/2026-07-22-plan-specific-collaboration-state.md)
- Pinned source: [.agents/notes/implemented/simplification/2026-07-22-plan-specific-collaboration-state.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/simplification/2026-07-22-plan-specific-collaboration-state.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`packages/plan/plan-mode/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/plan/plan-mode/src/index.ts) | package entry point | Entry point or contract under the directory named by the note: `packages/plan/plan-mode`. Core file in the package named by the note: `packages/plan/plan-mode`. | `named-directory-member, named-package-member, symbol-definition` |
| [`packages/plan/plan-mode/src/types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/plan/plan-mode/src/types.ts) | public types and contract | Entry point or contract under the directory named by the note: `packages/plan/plan-mode`. Core file in the package named by the note: `packages/plan/plan-mode`. | `named-directory-member, named-package-member` |
| [`packages/plan/plan-mode/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/plan/plan-mode/src/invariant.ts) | runtime contract checks | Entry point or contract under the directory named by the note: `packages/plan/plan-mode`. Core file in the package named by the note: `packages/plan/plan-mode`. | `named-directory-member, named-package-member` |
| [`packages/plan/plan-mode/README.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/plan/plan-mode/README.md) | package contract and examples | Entry point or contract under the directory named by the note: `packages/plan/plan-mode`. Core file in the package named by the note: `packages/plan/plan-mode`. | `named-directory-member, named-package-member` |
| [`packages/plan/plan-mode/package.json`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/plan/plan-mode/package.json) | composition and configuration | Entry point or contract under the directory named by the note: `packages/plan/plan-mode`. Core file in the package named by the note: `packages/plan/plan-mode`. | `named-directory-member, named-package-member` |
| [`packages/plan/plan-mode`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/packages/plan/plan-mode) | package or module directory | The source note names this implementation area directly. The note names this package or capability. | `named-directory, named-package` |
| [`packages/fs/tool-fs/src/edit.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/tool-fs/src/edit.ts) | runtime implementation | Defines `sandboxPolicy`, a construct named by the note. | `symbol-definition` |
| [`packages/core/session/src/surface.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/surface.ts) | runtime implementation | Defines `plan`, a construct named by the note. | `symbol-definition` |
| [`packages/client/ui-theme/src/client/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-theme/src/client/index.ts) | package entry point | Defines `modes`, a construct named by the note. | `symbol-definition` |
| [`packages/test-support/client-runtime/src/sessions.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/test-support/client-runtime/src/sessions.ts) | runtime implementation | Defines `set`, a construct named by the note. | `symbol-definition` |
| [`packages/client/ui-user-questions/src/client/QuestionComposer.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-user-questions/src/client/QuestionComposer.tsx) | runtime implementation | Defines `review`, a construct named by the note. | `symbol-definition` |
| [`docs/tool-catalog.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/tool-catalog.md) | package contract and examples | Contains the exact code literal `plan/mode` named by the note. | `exact-code-occurrence` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `modes` | `const` | [`packages/client/ui-theme/src/client/index.ts:355`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-theme/src/client/index.ts#L355) | `const modes = value as ThemeTokenModes` |
| `review` | `const` | [`packages/client/ui-user-questions/src/client/QuestionComposer.tsx:64`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-user-questions/src/client/QuestionComposer.tsx#L64) | `const review = useMemo(() => planReviewOf(question.questions), [question])` |
| `plan` | `const` | [`packages/core/session/src/surface.ts:357`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/surface.ts#L357) | `const plan = planSurfaceEvent(state, event, expectedSeq, events, baseSeq)` |
| `sandboxPolicy` | `const` | [`packages/fs/tool-fs/src/edit.ts:116`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/tool-fs/src/edit.ts#L116) | `const sandboxPolicy = await sandbox.resolvePolicy('edit', args, exec)` |
| `section` | `const` | [`packages/plan/plan-mode/src/index.ts:107`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/plan/plan-mode/src/index.ts#L107) | `const section = (config as Partial<PlanModeConfig>).section` |
| `foldPlanMode` | `function` | [`packages/plan/plan-mode/src/index.ts:129`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/plan/plan-mode/src/index.ts#L129) | `export function foldPlanMode(events: readonly SessionEvent[], end = events.length): boolean {` |
| `set` | `const` | [`packages/test-support/client-runtime/src/sessions.ts:52`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/test-support/client-runtime/src/sessions.ts#L52) | `const set = listeners.get(key) ?? new Set()` |

### Tests and executable evidence

- [`packages/plan/plan-mode/tests/plan-mode.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/plan/plan-mode/tests/plan-mode.spec.ts) — A test under the owning area exercises or imports `exit_plan_mode`. A test under the owning area exercises or imports `foldPlanMode`.
- [`packages/plan/plan-mode/tests/integration.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/plan/plan-mode/tests/integration.spec.ts) — A test under the owning area exercises or imports `exit_plan_mode`. A test under the owning area exercises or imports `foldPlanMode`.
- [`apps/web/tests/settings-chrome.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/settings-chrome.e2e.ts) — Contains the exact code literal `sandbox/mode` named by the note.
- [`apps/web/tests/snapshots/plan-review/session.jsonl`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/snapshots/plan-review/session.jsonl) — Contains the exact code literal `plan/mode` named by the note.
- [`packages/session/session-persistence/tests/persistence.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/session/session-persistence/tests/persistence.spec.ts) — Contains the exact code literal `mode/set` named by the note.
- Source verification intent: Package tests retain boundary ordering, retry, append-failure, HMR disposal, prompt assembly, stable native and Code Mode schemas, review outcomes, and invariant coverage through the boolean service. Command tests cover bare /plan, /plan , active /plan off, pending-entry cancellation, inactive idempotence, absence of /mode and /review, and effect-scoped removal. The keyless TUI scenarios enter through /plan , leave through /plan off, and prove that each committed plan/mode precedes the request header it changes, the entry message is logged under plan guidance, and the post-exit request omits that guidance.

## How to read the implementation

1. Start with [`packages/plan/plan-mode/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/plan/plan-mode/src/index.ts) because it has the strongest evidence link to the note.
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
- **`rust/context-evidence`** — Compaction may shrink presentation, but it must not erase the evidence ledger, file activity, or durable session facts.
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

- Tags: `class/simplification`, `concern/boundary`, `concern/cancellation-timeout`, `concern/compatibility`, `concern/discovery-routing`, `concern/evidence`, `concern/human-control`, `concern/lifecycle`, `concern/ownership`, `concern/recovery`, `concern/schema-types`, `concern/trust`, `domain/agent-loop`, `domain/build-release`, `domain/configuration`, `domain/context`, `domain/extensions`, `domain/filesystem`, `domain/llm`, `domain/protocols`, `domain/security`, `domain/session-state`, `domain/storage`, `domain/testing`, `domain/tools`, `domain/ui-interaction`, `lifecycle/implemented`, `mechanism/adapter`, `mechanism/policy`, `mechanism/projection`, `mechanism/registry`
- Aliases: `modes`, `review`, `plan`, `sandboxPolicy`, `section`, `foldPlanMode`, `set`, `ModeConfig.modes`, `ctx.modes.list`, `/plan`, `exit_plan_mode`, `ctx.sandboxPolicy`, `sandbox/mode`, `@deepseek-ai/dsh-plan-mode`
- Regex: `(?i)(modes|review|plan|sandboxPolicy|section|foldPlanMode|ModeConfig\.modes|ctx\.modes\.list)`

```bash
rg -n --pcre2 "(?i)(modes|review|plan|sandboxPolicy|section|foldPlanMode|ModeConfig\\.modes|ctx\\.modes\\.list)" source-deepseek-harness
rg -l --fixed-strings "class/simplification" 03-agent-context
```

## Connected notes

- **`source-link`** — [0459. ACP as an automation-only protocol](0459-acp-as-an-automation-only-protocol.md): The source note links to this decision directly.
- **`shares-code-with`** — [0256. Producer-declared context forms](0256-producer-declared-context-forms.md): Shares source implementation: `packages/plan/plan-mode/src/index.ts`, `packages/plan/plan-mode/src/invariant.ts`.
- **`shares-code-with`** — [0214. Plan review as a decision, not a question](0214-plan-review-as-a-decision-not-a-question.md): Shares source implementation: `packages/plan/plan-mode/src/index.ts`, `packages/plan/plan-mode/src/invariant.ts`.
- **`shares-code-with`** — [0074. dsh source launch through the tsx ESM hook](0074-dsh-source-launch-through-the-tsx-esm-hook.md): Shares source implementation: `packages/plan/plan-mode/src/index.ts`, `packages/plan/plan-mode/src/invariant.ts`.
- **`shares-code-with`** — [0133. Compaction as a capability seam (abstract contract + basic backend)](0133-compaction-as-a-capability-seam-abstract-contract-basic-backend.md): Shares source implementation: `packages/core/session/src/surface.ts`.
- **`same-design-pressure`** — [0146. The subprocess sandbox --- confinement seam, native runners, escalation, and per-session modes](0146-the-subprocess-sandbox-confinement-seam-native-runners-escalation-and-pe.md): Shares design concerns: `concern/boundary`, `concern/compatibility`, `concern/discovery-routing`.
- **`same-design-pressure`** — [0126. Repository naming contract and pre-release rename ledger](0126-repository-naming-contract-and-pre-release-rename-ledger.md): Shares design concerns: `concern/boundary`, `concern/cancellation-timeout`, `concern/compatibility`.
- **`shares-code-with`** — [0063. Web input state machine, composer slots, and the slash pipeline (ui-conversation input / ui-input-trigger)](0063-web-input-state-machine-composer-slots-and-the-slash-pipeline-ui-convers.md): Shares source implementation: `packages/core/session/src/surface.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0458-plan-specific-collaboration-state.md`.
