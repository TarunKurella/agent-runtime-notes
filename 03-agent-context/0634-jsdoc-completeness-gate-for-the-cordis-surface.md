---
id: "dsh-note-0634"
title: "JSDoc completeness gate for the cordis surface"
status: "archived"
class: "process"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/archived/process/2026-07-04-cordis-jsdoc-completeness-gate.md"
implementation_evidence: "high"
target_anchor: "repository tests and engineering policy"
tags:
  - "class/process"
  - "concern/boundary"
  - "concern/evidence"
  - "domain/build-release"
  - "domain/extensions"
  - "domain/llm"
  - "lifecycle/archived"
  - "mechanism/policy"
  - "mechanism/projection"
aliases:
  - "next"
  - "collectEvents"
  - "collectServices"
  - "parseJsDoc"
  - "interface Events"
  - "interface Context"
  - "scripts/gen-cordis-catalog.ts"
  - "verify-cordis-catalog"
  - "doc-sync"
  - "ts cordis-catalog"
  - "packages/core/agent/tests/gen-cordis-catalog.spec.ts"
  - "ctx.<key>"
  - "JSDoc completeness gate for the cordis surface"
  - "process"
search_regex: "(?i)(next|collectEvents|collectServices|parseJsDoc|interface[- ]Events|interface[- ]Context|scripts/gen\\-cordis\\-catalog\\.ts|verify\\-cordis\\-catalog)"
---

# 0634. JSDoc completeness gate for the cordis surface — implementation context

## Open this when

The generated Cordis catalog enforced event dispatch modes but not complete service and event contracts. Methods could lack descriptions, and parameters or returns could be undocumented on the cross-plugin API surface where IDE guidance matters most. The AGENTS.md rule ("every export has a JSDoc explaining semantics") is prose-checkable only by review; the repo's stated preference is to encode invariants in mechanical gates.

## Source decision

Extend scripts/gen-cordis-catalog.ts --- the same walk, the same @mode precedent --- to enforce JSDoc COMPLETENESS on everything it catalogs. verify-cordis-catalog runs inside doc-sync, so relevant documentation changes and CI exercise the same gate without separate wiring. The contract: Events need description prose plus a non-empty @param for every payload parameter.

## Decision status

Historical design. Use it to understand an older pressure or failure; locate the replacing decision before implementing anything.

- Raw note: [archived/process/2026-07-04-cordis-jsdoc-completeness-gate.md](../02-notes/archived/process/2026-07-04-cordis-jsdoc-completeness-gate.md)
- Pinned source: [.agents/notes/archived/process/2026-07-04-cordis-jsdoc-completeness-gate.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/archived/process/2026-07-04-cordis-jsdoc-completeness-gate.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`AGENTS.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/AGENTS.md) | package contract and examples | The source note names this file directly. | `named-file` |
| [`scripts/gen-cordis-catalog.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/gen-cordis-catalog.ts) | repository automation | The source note names this file directly. | `named-file` |
| [`scripts/jsdoc.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/jsdoc.ts) | repository automation | Defines `parseJsDoc`, a construct named by the note. | `symbol-definition` |
| [`vendor/loader/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/loader/src/index.ts) | package entry point | Defines `next`, a construct named by the note. | `symbol-definition` |
| [`vendor/cordis/src/events.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/cordis/src/events.ts) | runtime implementation | Defines `next`, a construct named by the note. | `symbol-definition` |
| [`vendor/cosmokit/src/string.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/cosmokit/src/string.ts) | runtime implementation | Defines `next`, a construct named by the note. | `symbol-definition` |
| [`packages/llm/llm/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/src/index.ts) | package entry point | Defines `next`, a construct named by the note. | `symbol-definition` |
| [`packages/goal/goal/src/fold.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/goal/goal/src/fold.ts) | runtime implementation | Defines `next`, a construct named by the note. | `symbol-definition` |
| [`packages/typert/generator/src/cordis-catalog.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/typert/generator/src/cordis-catalog.ts) | runtime implementation | Defines `parseJsDoc`, a construct named by the note. Defines `collectEvents`, a construct named by the note. | `symbol-definition` |
| [`package.json`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/package.json) | composition and configuration | Contains the exact code literal `scripts/gen-cordis-catalog.ts` named by the note. | `exact-code-occurrence` |
| [`docs/cordis-api/events.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/cordis-api/events.md) | package contract and examples | Contains the exact code literal `scripts/gen-cordis-catalog.ts` named by the note. | `exact-code-occurrence` |
| [`scripts/cordis-core-api.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/cordis-core-api.ts) | repository automation | Contains the exact code literal `scripts/gen-cordis-catalog.ts` named by the note. | `exact-code-occurrence` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `next` | `const` | [`packages/goal/goal/src/fold.ts:205`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/goal/goal/src/fold.ts#L205) | `const next = change.goal` |
| `next` | `const` | [`packages/llm/llm/src/index.ts:877`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/src/index.ts#L877) | `const next = await iterator.next()` |
| `collectEvents` | `function` | [`packages/typert/generator/src/cordis-catalog.ts:403`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/typert/generator/src/cordis-catalog.ts#L403) | `export function collectEvents(scanRoot: string, policy: CordisCatalogPolicy): EventEntry[] {` |
| `collectServices` | `function` | [`packages/typert/generator/src/cordis-catalog.ts:413`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/typert/generator/src/cordis-catalog.ts#L413) | `export function collectServices(scanRoot: string, policy: CordisCatalogPolicy): ServiceEntry[] {` |
| `parseJsDoc` | `function` | [`packages/typert/generator/src/cordis-catalog.ts:425`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/typert/generator/src/cordis-catalog.ts#L425) | `function parseJsDoc(raw: string): ParsedJsDoc {` |
| `parseJsDoc` | `function` | [`scripts/jsdoc.ts:32`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/jsdoc.ts#L32) | `export function parseJsDoc(raw: string): { doc: string; mode: Mode \| null; hasMode: boolean } {` |
| `next` | `const` | [`vendor/cordis/src/events.ts:237`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/cordis/src/events.ts#L237) | `const next = () => {` |
| `next` | `const` | [`vendor/cosmokit/src/string.ts:29`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/cosmokit/src/string.ts#L29) | `const next = source.charCodeAt(i + 1)` |
| `next` | `const` | [`vendor/loader/src/index.ts:181`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/loader/src/index.ts#L181) | `const next = fiber.parent.fiber` |

### Tests and executable evidence

- [`scripts/clean.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/clean.spec.ts) — A test under the owning area exercises or imports `void`.
- [`scripts/dev-web.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/dev-web.spec.ts) — A test under the owning area exercises or imports `Promise`.
- [`scripts/run-gates.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/run-gates.spec.ts) — A test under the owning area exercises or imports `doc-sync`.
- [`scripts/test-invariants.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/test-invariants.ts) — A test under the owning area exercises or imports `this`. A test under the owning area exercises or imports `void`.
- [`scripts/ci-workflow.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/ci-workflow.spec.ts) — A test under the owning area exercises or imports `this`.
- [`scripts/rescope-vendor.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/rescope-vendor.spec.ts) — A test under the owning area exercises or imports `this`.
- [`scripts/test-invariants.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/test-invariants.spec.ts) — A test under the owning area exercises or imports `void`. A test under the owning area exercises or imports `Promise`.
- [`scripts/oxlint-contract.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/oxlint-contract.spec.ts) — A test under the owning area exercises or imports `Promise`.

## How to read the implementation

1. Start with [`AGENTS.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/AGENTS.md) because it has the strongest evidence link to the note.
2. Read public types and entry points before implementation details. Write down the contract in plain language.
3. Follow the named constructs into their callers and tests. Confirm which component can actually enforce the rule.
4. Reconstruct one failure path: cancellation, crash, retry, stale state, partial write, or incompatible provider input—whichever the note is protecting against.
5. Only then map the contract into the current Rust repository. Target paths must come from that repository, never from guesswork in this wiki.

## Rust harness port

**Likely integration point:** repository tests and engineering policy.

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

- Tags: `class/process`, `concern/boundary`, `concern/evidence`, `domain/build-release`, `domain/extensions`, `domain/llm`, `lifecycle/archived`, `mechanism/policy`, `mechanism/projection`
- Aliases: `next`, `collectEvents`, `collectServices`, `parseJsDoc`, `interface Events`, `interface Context`, `scripts/gen-cordis-catalog.ts`, `verify-cordis-catalog`, `doc-sync`, `ts cordis-catalog`, `packages/core/agent/tests/gen-cordis-catalog.spec.ts`, `ctx.<key>`, `JSDoc completeness gate for the cordis surface`, `process`
- Regex: `(?i)(next|collectEvents|collectServices|parseJsDoc|interface[- ]Events|interface[- ]Context|scripts/gen\-cordis\-catalog\.ts|verify\-cordis\-catalog)`

```bash
rg -n --pcre2 "(?i)(next|collectEvents|collectServices|parseJsDoc|interface[- ]Events|interface[- ]Context|scripts/gen\\-cordis\\-catalog\\.ts|verify\\-cordis\\-catalog)" source-deepseek-harness
rg -l --fixed-strings "class/process" 03-agent-context
```

## Connected notes

- **`shares-code-with`** — [0388. Export JSDoc gate](0388-export-jsdoc-gate.md): Shares source implementation: `packages/llm/llm/src/index.ts`, `scripts/jsdoc.ts`.
- **`shares-code-with`** — [0088. Claim inbox input before one pre-step decision](0088-claim-inbox-input-before-one-pre-step-decision.md): Shares source implementation: `packages/goal/goal/src/fold.ts`, `packages/llm/llm/src/index.ts`.
- **`shares-code-with`** — [0058. Separate context injection from turn execution](0058-separate-context-injection-from-turn-execution.md): Shares source implementation: `packages/goal/goal/src/fold.ts`, `packages/llm/llm/src/index.ts`.
- **`shares-code-with`** — [0454. Simplify session-log representation](0454-simplify-session-log-representation.md): Shares source implementation: `packages/goal/goal/src/fold.ts`, `packages/llm/llm/src/index.ts`.
- **`shares-code-with`** — [0078. Terminal LLM stream failures](0078-terminal-llm-stream-failures.md): Shares source implementation: `packages/goal/goal/src/fold.ts`, `packages/llm/llm/src/index.ts`.
- **`shares-code-with`** — [0632. Generated cordis events + services catalog](0632-generated-cordis-events-services-catalog.md): Shares source implementation: `AGENTS.md`, `scripts/gen-cordis-catalog.ts`.
- **`shares-code-with`** — [0121. Remote event delivery (ctx.remote.$on)](0121-remote-event-delivery-ctx-remote-on.md): Shares source implementation: `packages/llm/llm/src/index.ts`, `vendor/cordis/src/events.ts`.
- **`shares-code-with`** — [0651. Drop unconsumed assembled LLM convenience surfaces](0651-drop-unconsumed-assembled-llm-convenience-surfaces.md): Shares source implementation: `AGENTS.md`, `packages/llm/llm/src/index.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0634-jsdoc-completeness-gate-for-the-cordis-surface.md`.
