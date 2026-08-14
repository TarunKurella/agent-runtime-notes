---
id: "dsh-note-0641"
title: "Tutorial-style Cordis docs under docs/cordis-tutorial"
status: "archived"
class: "process"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/archived/process/2026-07-22-cordis-tutorial-docs.md"
implementation_evidence: "high"
target_anchor: "repository tests and engineering policy"
tags:
  - "class/process"
  - "concern/boundary"
  - "concern/discovery-routing"
  - "concern/evidence"
  - "concern/human-control"
  - "concern/lifecycle"
  - "concern/schema-types"
  - "domain/configuration"
  - "domain/extensions"
  - "domain/llm"
  - "domain/session-state"
  - "domain/storage"
  - "domain/tools"
  - "domain/ui-interaction"
  - "lifecycle/archived"
  - "mechanism/policy"
  - "mechanism/projection"
aliases:
  - "defineTool"
  - "CallId"
  - "mirroredPages"
  - "docs/user/develop/"
  - "docs/cordis-tutorial/"
  - "tmp/cordis-tutorial/"
  - "node --import tsx ../../vendor/cordis/bin.js"
  - "@deepseek-ai/dsh-tools"
  - "@deepseek-ai/dsh-llm"
  - "ctx.tools"
  - "tools/result"
  - "doc-typecheck"
  - "./stats.ts"
  - "ignore-check"
search_regex: "(?i)(defineTool|CallId|mirroredPages|docs/user/develop/|docs/cordis\\-tutorial/|tmp/cordis\\-tutorial/|node[- ]\\-\\-import[- ]tsx[- ]\\.\\./\\.\\./vendor/cordis/bin\\.js|@deepseek\\-ai/dsh\\-tools)"
---

# 0641. Tutorial-style Cordis docs under docs/cordis-tutorial — implementation context

## Open this when

The repo documents Cordis at two levels: the condensed cordis-primer states the concepts, and the docs/user/develop/ pages teach harness plugin authoring against harness services. Neither serves a developer meeting Cordis itself for the first time: the primer assumes the reader already writes plugins, and the develop pages jump straight to defineTool without showing how contexts, fibers, services, and dispatch actually behave. There was no path where a reader runs bare Cordis, watches a fiber go PENDING, or sees a waterfall veto happen.

## Source decision

docs/cordis-tutorial/ holds a seven-chapter hands-on tutorial (first plugin → lifecycle/effects → services → events → config → composition/HMR → harness tool). Its properties, in decreasing order of load-bearing-ness: Every transcript is real. Each chapter's files run in the gitignored tmp/cordis-tutorial/ scratch directory via node --import tsx ../../vendor/cordis/bin.js, and the shown output is what those commands print. The chapter that uses harness packages (@deepseek-ai/dsh-tools and @deepseek-ai/dsh-llm) runs keylessly.

## Decision status

Historical design. Use it to understand an older pressure or failure; locate the replacing decision before implementing anything.

- Raw note: [archived/process/2026-07-22-cordis-tutorial-docs.md](../02-notes/archived/process/2026-07-22-cordis-tutorial-docs.md)
- Pinned source: [.agents/notes/archived/process/2026-07-22-cordis-tutorial-docs.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/archived/process/2026-07-22-cordis-tutorial-docs.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`website/docs.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/website/docs.ts) | runtime implementation | The source note names this file directly. Defines `mirroredPages`, a construct named by the note. | `exact-code-occurrence, named-file, symbol-definition` |
| [`vendor/cordis/bin.js`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/cordis/bin.js) | runtime implementation | The source note names this file directly. | `named-file` |
| [`docs/cordis-primer.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/cordis-primer.md) | package contract and examples | The source note names this file directly. | `named-file` |
| [`packages/llm/llm/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/src/index.ts) | package entry point | Core file in the package named by the note: `packages/llm/llm`. | `named-package-member` |
| [`packages/llm/llm/src/types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/src/types.ts) | public types and contract | Core file in the package named by the note: `packages/llm/llm`. | `named-package-member` |
| [`packages/llm/llm/src/brand.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/src/brand.ts) | runtime implementation | Core file in the package named by the note: `packages/llm/llm`. Defines `CallId`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/core/tools/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/index.ts) | package entry point | Core file in the package named by the note: `packages/core/tools`. | `named-package-member` |
| [`packages/core/tools/src/types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/types.ts) | public types and contract | Core file in the package named by the note: `packages/core/tools`. | `named-package-member` |
| [`packages/core/tools/src/schema.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/schema.ts) | runtime implementation | Core file in the package named by the note: `packages/core/tools`. Defines `defineTool`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/llm/llm/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/llm/llm`. | `named-package-member` |
| [`packages/core/tools/src/testing.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/testing.ts) | runtime implementation | Core file in the package named by the note: `packages/core/tools`. | `named-package-member` |
| [`packages/core/tools/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/core/tools`. | `named-package-member` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `defineTool` | `function` | [`packages/core/tools/src/schema.ts:545`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/schema.ts#L545) | `export function defineTool<const S extends ParameterSchemaSpec, const O extends ValueSchemaSpec>(` |
| `CallId` | `type` | [`packages/llm/llm/src/brand.ts:31`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/src/brand.ts#L31) | `export type CallId = Branded<'CallId'>` |
| `CallId` | `function` | [`packages/llm/llm/src/brand.ts:38`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/src/brand.ts#L38) | `export function CallId(id: string): CallId {` |
| `mirroredPages` | `function` | [`website/docs.ts:71`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/website/docs.ts#L71) | `function mirroredPages(pages: MirroredPage[]): DocsPage[] {` |

### Tests and executable evidence

- [`packages/core/tools/tests/tools.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/tests/tools.spec.ts) — A test under the owning area exercises or imports `defineTool`.
- [`packages/core/tools/tests/py-types.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/tests/py-types.spec.ts) — A test under the owning area exercises or imports `defineTool`.
- [`packages/core/tools/tests/code-mode.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/tests/code-mode.spec.ts) — A test under the owning area exercises or imports `defineTool`.
- [`packages/core/tools/tests/execution-signal-types.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/tests/execution-signal-types.spec.ts) — A test under the owning area exercises or imports `defineTool`.

## How to read the implementation

1. Start with [`website/docs.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/website/docs.ts) because it has the strongest evidence link to the note.
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
- **`rust/quickjs-boundary`** — QuickJS holds orchestration variables; Rust/Tokio owns async execution and exposes only explicitly granted capabilities.
- **`rust/serde-validation`** — Parse untrusted input into strict serde types and validate semantic constraints before executing side effects.
- **`rust/tagged-enums`** — Use serde-tagged enums for a closed wire vocabulary. Add an explicit extension envelope only where runtime extension is truly required.
- **`rust/trait-seam`** — Use a small trait at the capability boundary; keep policy and model-facing schemas outside the backend implementation.
- **Status gate** — Historical design. Use it to understand an older pressure or failure; locate the replacing decision before implementing anything.

### Smallest safe implementation slice

1. Express the semantic contract as Rust types and one owning boundary.
2. Add the smallest adapter or service needed by the existing loop; do not add a new model-facing tool unless the four-tool surface cannot express the job.
3. Record durable evidence before deriving summaries or UI state.
4. Add one happy-path test, one interruption test, and one replay/restart test where state survives a turn.
5. Keep the patch reversible until the source note's strongest failure case is proven in the Rust harness.

## Search handles

- Tags: `class/process`, `concern/boundary`, `concern/discovery-routing`, `concern/evidence`, `concern/human-control`, `concern/lifecycle`, `concern/schema-types`, `domain/configuration`, `domain/extensions`, `domain/llm`, `domain/session-state`, `domain/storage`, `domain/tools`, `domain/ui-interaction`, `lifecycle/archived`, `mechanism/policy`, `mechanism/projection`
- Aliases: `defineTool`, `CallId`, `mirroredPages`, `docs/user/develop/`, `docs/cordis-tutorial/`, `tmp/cordis-tutorial/`, `node --import tsx ../../vendor/cordis/bin.js`, `@deepseek-ai/dsh-tools`, `@deepseek-ai/dsh-llm`, `ctx.tools`, `tools/result`, `doc-typecheck`, `./stats.ts`, `ignore-check`
- Regex: `(?i)(defineTool|CallId|mirroredPages|docs/user/develop/|docs/cordis\-tutorial/|tmp/cordis\-tutorial/|node[- ]\-\-import[- ]tsx[- ]\.\./\.\./vendor/cordis/bin\.js|@deepseek\-ai/dsh\-tools)`

```bash
rg -n --pcre2 "(?i)(defineTool|CallId|mirroredPages|docs/user/develop/|docs/cordis\\-tutorial/|tmp/cordis\\-tutorial/|node[- ]\\-\\-import[- ]tsx[- ]\\.\\./\\.\\./vendor/cordis/bin\\.js|@deepseek\\-ai/dsh\\-tools)" source-deepseek-harness
rg -l --fixed-strings "class/process" 03-agent-context
```

## Connected notes

- **`shares-code-with`** — [0145. Explicit model-facing tool order](0145-explicit-model-facing-tool-order.md): Shares source implementation: `packages/core/tools/src/index.ts`, `packages/core/tools/src/types.ts`.
- **`shares-code-with`** — [0006. Structured error taxonomy](0006-structured-error-taxonomy.md): Shares source implementation: `packages/core/tools/src/index.ts`, `packages/core/tools/src/schema.ts`.
- **`shares-code-with`** — [0578. The /reload command re-reads loader configs on demand](0578-the-reload-command-re-reads-loader-configs-on-demand.md): Shares source implementation: `packages/core/tools/src/index.ts`, `packages/core/tools/src/types.ts`.
- **`shares-code-with`** — [0149. The self-referential cordis toolset](0149-the-self-referential-cordis-toolset.md): Shares source implementation: `docs/cordis-primer.md`, `packages/core/tools/src/index.ts`.
- **`shares-code-with`** — [0131. Code Mode --- the model writes TypeScript against the tool registry](0131-code-mode-the-model-writes-typescript-against-the-tool-registry.md): Shares source implementation: `packages/core/tools/src/index.ts`, `packages/core/tools/src/types.ts`.
- **`shares-code-with`** — [0494. Property-based testing for protocol-shaped code](0494-property-based-testing-for-protocol-shaped-code.md): Shares source implementation: `packages/core/tools/src/index.ts`, `packages/core/tools/src/types.ts`.
- **`shares-code-with`** — [0018. Web capability seam - stable tools over multiple providers](0018-web-capability-seam-stable-tools-over-multiple-providers.md): Shares source implementation: `packages/core/tools/src/index.ts`, `packages/core/tools/src/types.ts`.
- **`shares-code-with`** — [0071. Create every message as an identified immutable value](0071-create-every-message-as-an-identified-immutable-value.md): Shares source implementation: `packages/llm/llm/src/brand.ts`, `packages/llm/llm/src/index.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0641-tutorial-style-cordis-docs-under-docs-cordis-tutorial.md`.
