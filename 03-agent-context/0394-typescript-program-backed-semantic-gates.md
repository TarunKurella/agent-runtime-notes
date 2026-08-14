---
id: "dsh-note-0394"
title: "TypeScript Program-backed semantic gates"
status: "implemented"
class: "process"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/process/2026-07-14-typescript-program-backed-semantic-gates.md"
implementation_evidence: "high"
target_anchor: "repository tests and engineering policy"
tags:
  - "class/process"
  - "concern/boundary"
  - "concern/discovery-routing"
  - "concern/evidence"
  - "concern/lifecycle"
  - "concern/ownership"
  - "concern/performance"
  - "concern/recovery"
  - "concern/schema-types"
  - "domain/build-release"
  - "domain/configuration"
  - "domain/extensions"
  - "domain/filesystem"
  - "domain/llm"
  - "domain/multi-agent"
  - "domain/session-state"
  - "domain/testing"
  - "lifecycle/implemented"
  - "mechanism/adapter"
  - "mechanism/generation"
  - "mechanism/policy"
  - "mechanism/projection"
  - "mechanism/registry"
aliases:
  - "json"
  - "AgentEventDispatch"
  - "scopeTarget"
  - "Context"
  - "TypeScriptProject"
  - "EventsService"
  - "Events"
  - "ts.Program"
  - "TypeChecker"
  - "tsconfig.json"
  - "gen-doc-graphs"
  - "EventsService.dispatch"
  - "getResolvedSignature"
  - "internal/dispatch"
search_regex: "(?i)(json|AgentEventDispatch|scopeTarget|Context|TypeScriptProject|EventsService|Events|ts\\.Program)"
---

# 0394. TypeScript Program-backed semantic gates — implementation context

## Open this when

Repository gates sometimes need facts that TypeScript syntax does not carry by itself: whether a receiver is a Cordis Context, which concrete event names reach a forwarding helper, and whether declaration merging changed an event signature. The existing gates use TypeScript's single-file syntax model and maintain these facts through naming conventions, handwritten tables, and JSDoc. The repository needs one semantic source of truth without introducing runtime package cycles, broad fallback heuristics, or machine-readable annotations that restate information already available to TypeScript.

## Source decision

Repository gates can combine project-wide type information through ts.Program and use TypeChecker to extract strongly typed facts, reducing their reliance on naming conventions, handwritten tables, and JSDoc metadata. The repository applies this model to two gates. TypeScriptProject parses the root tsconfig.json, recursively expands every project reference, and combines the referenced source roots into one no-emit semantic program.

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/process/2026-07-14-typescript-program-backed-semantic-gates.md](../02-notes/implemented/process/2026-07-14-typescript-program-backed-semantic-gates.md)
- Pinned source: [.agents/notes/implemented/process/2026-07-14-typescript-program-backed-semantic-gates.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/process/2026-07-14-typescript-program-backed-semantic-gates.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`scripts/ts-project.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/ts-project.ts) | repository automation | The source note names this file directly. Defines `TypeScriptProject`, a construct named by the note. | `named-file, symbol-definition` |
| [`scripts/gen-doc-graphs.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/gen-doc-graphs.ts) | repository automation | The source note names this file directly. | `named-file` |
| [`scripts/gen-scoped-events.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/gen-scoped-events.ts) | repository automation | The source note names this file directly. Contains the exact code literal `dsh-scope` named by the note. | `exact-code-occurrence, named-file` |
| [`packages/core/scope/src/scoped-events.generated.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/scope/src/scoped-events.generated.ts) | runtime implementation | The source note names this file directly. | `named-file` |
| [`packages/core/scope/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/scope/src/index.ts) | package entry point | Core file in the package named by the note: `packages/core/scope`. Defines `scopeTarget`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/core/scope/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/scope/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/core/scope`. | `named-package-member` |
| [`packages/runtime-diagnostics/invariants/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/runtime-diagnostics/invariants/src/index.ts) | package entry point | Core file in the package named by the note: `packages/runtime-diagnostics/invariants`. Defines `Context`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/runtime-diagnostics/invariants/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/runtime-diagnostics/invariants/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/runtime-diagnostics/invariants`. | `named-package-member` |
| [`packages/core/scope`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/packages/core/scope) | package or module directory | The note names this package or capability. | `named-package` |
| [`packages/runtime-diagnostics/invariants`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/packages/runtime-diagnostics/invariants) | package or module directory | The note names this package or capability. | `named-package` |
| [`vendor/hmr/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/hmr/src/index.ts) | package entry point | Defines `Events`, a construct named by the note. | `symbol-definition` |
| [`vendor/cordis/src/events.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/cordis/src/events.ts) | runtime implementation | Defines `EventsService`, a construct named by the note. Contains the exact code literal `internal/dispatch` named by the note. | `exact-code-occurrence, symbol-definition` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `json` | `const` | [`packages/client/modules/src/index.ts:169`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/modules/src/index.ts#L169) | `const json = JSON.stringify(graph).replaceAll('<', '\\u003c')` |
| `AgentEventDispatch` | `interface` | [`packages/core/agent/src/dispatch.ts:54`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent/src/dispatch.ts#L54) | `export interface AgentEventDispatch {` |
| `scopeTarget` | `function` | [`packages/core/scope/src/index.ts:170`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/scope/src/index.ts#L170) | `export function scopeTarget<T extends object>(base: T, key: ScopeKey \| undefined): Scoped<T> {` |
| `Context` | `interface` | [`packages/runtime-diagnostics/invariants/src/index.ts:69`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/runtime-diagnostics/invariants/src/index.ts#L69) | `interface Context {` |
| `TypeScriptProject` | `class` | [`scripts/ts-project.ts:78`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/ts-project.ts#L78) | `export class TypeScriptProject {` |
| `EventsService` | `class` | [`vendor/cordis/src/events.ts:131`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/cordis/src/events.ts#L131) | `export class EventsService {` |
| `Events` | `interface` | [`vendor/hmr/src/index.ts:20`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/hmr/src/index.ts#L20) | `interface Events {` |

### Tests and executable evidence

- [`packages/core/scope/tests/scope.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/scope/tests/scope.spec.ts) — A test under the owning area exercises or imports `scopeTarget`.
- [`packages/core/scope/tests/invariant.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/scope/tests/invariant.spec.ts) — A test under the owning area exercises or imports `scopeTarget`.
- [`scripts/test-invariants.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/test-invariants.ts) — Contains the exact code literal `dsh-invariants` named by the note.
- [`scripts/test-invariants.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/test-invariants.spec.ts) — Contains the exact code literal `dsh-invariants` named by the note.
- [`scripts/package-invariants.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/package-invariants.spec.ts) — Contains the exact code literal `dsh-invariants` named by the note.
- [`packages/core/tools/tests/scoped.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/tests/scoped.spec.ts) — Contains the exact code literal `internal/dispatch` named by the note.
- [`packages/core/agent-loop/tests/coverage-edges.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent-loop/tests/coverage-edges.spec.ts) — Contains the exact code literal `internal/dispatch` named by the note.
- [`packages/core/agent-loop/tests/contract-regressions.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent-loop/tests/contract-regressions.spec.ts) — Contains the exact code literal `internal/dispatch` named by the note.
- Source verification intent: verify-doc-graphs freshness-checks semantic producer/listener discovery, and verify-scoped-events reruns the Program analysis while freshness-checking the generated resolver map. The root TypeScript build compiles its runtime adapter; workspace constraints and runtime-closure checks keep event-owner aggregation out of deployment dependencies.

## How to read the implementation

1. Start with [`scripts/ts-project.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/ts-project.ts) because it has the strongest evidence link to the note.
2. Read public types and entry points before implementation details. Write down the contract in plain language.
3. Follow the named constructs into their callers and tests. Confirm which component can actually enforce the rule.
4. Reconstruct one failure path: cancellation, crash, retry, stale state, partial write, or incompatible provider input—whichever the note is protecting against.
5. Only then map the contract into the current Rust repository. Target paths must come from that repository, never from guesswork in this wiki.

## Rust harness port

**Likely integration point:** repository tests and engineering policy.

Preserve the behavior and ownership rule. Do not mechanically translate Cordis, TypeScript declaration merging, Node streams, or package topology into Rust.

Relevant standing rules:

- **`rust/append-log`** — Keep durable facts append-only and derive views from them. Separate the synchronous fact boundary from asynchronous persistence.
- **`rust/capability-security`** — Make granted capabilities explicit. On the Windows no-admin path, probe support at runtime and fail closed rather than silently running unsandboxed.
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

- Tags: `class/process`, `concern/boundary`, `concern/discovery-routing`, `concern/evidence`, `concern/lifecycle`, `concern/ownership`, `concern/performance`, `concern/recovery`, `concern/schema-types`, `domain/build-release`, `domain/configuration`, `domain/extensions`, `domain/filesystem`, `domain/llm`, `domain/multi-agent`, `domain/session-state`, `domain/testing`, `lifecycle/implemented`, `mechanism/adapter`, `mechanism/generation`, `mechanism/policy`, `mechanism/projection`, `mechanism/registry`
- Aliases: `json`, `AgentEventDispatch`, `scopeTarget`, `Context`, `TypeScriptProject`, `EventsService`, `Events`, `ts.Program`, `TypeChecker`, `tsconfig.json`, `gen-doc-graphs`, `EventsService.dispatch`, `getResolvedSignature`, `internal/dispatch`
- Regex: `(?i)(json|AgentEventDispatch|scopeTarget|Context|TypeScriptProject|EventsService|Events|ts\.Program)`

```bash
rg -n --pcre2 "(?i)(json|AgentEventDispatch|scopeTarget|Context|TypeScriptProject|EventsService|Events|ts\\.Program)" source-deepseek-harness
rg -l --fixed-strings "class/process" 03-agent-context
```

## Connected notes

- **`shares-code-with`** — [0061. Web client Agent-scope parity model and the provisioning channel (agents/scope / blank reuse / provide)](0061-web-client-agent-scope-parity-model-and-the-provisioning-channel-agents.md): Shares source implementation: `packages/core/scope`, `packages/core/scope/src/index.ts`.
- **`shares-code-with`** — [0052. The slot system standard --- single register, four props shares, and the framework store seat](0052-the-slot-system-standard-single-register-four-props-shares-and-the-frame.md): Shares source implementation: `packages/core/scope`, `packages/core/scope/src/index.ts`.
- **`shares-code-with`** — [0201. Cross-workspace session resume](0201-cross-workspace-session-resume.md): Shares source implementation: `packages/core/scope`, `packages/core/scope/src/index.ts`.
- **`shares-code-with`** — [0173. Durable subagent catalog and list_agents](0173-durable-subagent-catalog-and-list-agents.md): Shares source implementation: `packages/core/scope/src/index.ts`, `packages/core/scope/src/invariant.ts`.
- **`shares-code-with`** — [0035. Agent-scope runtime design and correctness](0035-agent-scope-runtime-design-and-correctness.md): Shares source implementation: `packages/core/scope/src/index.ts`, `packages/core/scope/src/invariant.ts`.
- **`shares-code-with`** — [0116. The skill registry is host-held and layered per scope](0116-the-skill-registry-is-host-held-and-layered-per-scope.md): Shares source implementation: `packages/core/scope`, `packages/core/scope/src/index.ts`.
- **`shares-code-with`** — [0047. Package-owned invariant service contract](0047-package-owned-invariant-service-contract.md): Shares source implementation: `packages/core/scope/src/index.ts`, `packages/core/scope/src/invariant.ts`.
- **`shares-code-with`** — [0036. Shared scoped-layer storage](0036-shared-scoped-layer-storage.md): Shares source implementation: `packages/core/scope/src/index.ts`, `packages/core/scope/src/invariant.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0394-typescript-program-backed-semantic-gates.md`.
