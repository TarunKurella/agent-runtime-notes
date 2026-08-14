---
id: "dsh-note-0655"
title: "Drop the unconsumed web observation surface --- the `providers-change` event and the status methods"
status: "archived"
class: "simplification"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/archived/simplification/2026-07-04-drop-unconsumed-web-observation-surface.md"
implementation_evidence: "high"
target_anchor: "Rust provider-adapter boundary"
tags:
  - "class/simplification"
  - "concern/boundary"
  - "concern/discovery-routing"
  - "concern/evidence"
  - "concern/ownership"
  - "concern/performance"
  - "concern/simplification"
  - "domain/extensions"
  - "domain/llm"
  - "domain/observability"
  - "domain/session-state"
  - "domain/testing"
  - "lifecycle/archived"
  - "mechanism/adapter"
  - "mechanism/projection"
  - "mechanism/registry"
aliases:
  - "WebError"
  - "providers-change"
  - "WebService"
  - "web/providers-change"
  - "packages/web/web/src/index.ts"
  - "searchStatus"
  - "fetchStatus"
  - "WebCapabilityStatus"
  - "dsh-tool-web"
  - "ctx.web.search"
  - "packages/web/tool-web/src/search.ts"
  - "packages/web/tool-web/src/fetch.ts"
  - "packages/web/tool-web/README.md"
  - "packages/web/tool-web/src/index.ts"
search_regex: "(?i)(WebError|providers\\-change|WebService|web/providers\\-change|packages/web/web/src/index\\.ts|searchStatus|fetchStatus|WebCapabilityStatus)"
---

# 0655. Drop the unconsumed web observation surface --- the `providers-change` event and the status methods — implementation context

## Open this when

WebService exposes an observation surface no production code observes: web/providers-change (packages/web/web/src/index.ts) is declared and emitted on every provider registration and disposal, and each registration effect's rollback yield is ordered BEFORE the emit solely so a throwing change listener unwinds the registration. No listener exists outside the package's own two unit tests (one of which exists to pin that rollback ordering).

## Source decision

Remove the registry-change event, aggregated status methods and type, and their dedicated tests. Provider-private status remains for execution-time selection. Caller-facing coverage now asserts successful execution or structured selection errors, and the owning web docs describe that on-call contract.

## Decision status

Historical design. Use it to understand an older pressure or failure; locate the replacing decision before implementing anything.

- Raw note: [archived/simplification/2026-07-04-drop-unconsumed-web-observation-surface.md](../02-notes/archived/simplification/2026-07-04-drop-unconsumed-web-observation-surface.md)
- Pinned source: [.agents/notes/archived/simplification/2026-07-04-drop-unconsumed-web-observation-surface.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/archived/simplification/2026-07-04-drop-unconsumed-web-observation-surface.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`docs/architecture.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/architecture.md) | package contract and examples | The source note names this file directly. | `named-file` |
| [`packages/web/web/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/web/web/src/index.ts) | package entry point | The source note names this file directly. | `named-file` |
| [`packages/web/tool-web/README.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/web/tool-web/README.md) | package contract and examples | The source note names this file directly. Core file in the package named by the note: `packages/web/tool-web`. | `named-file, named-package-member` |
| [`packages/web/tool-web/src/fetch.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/web/tool-web/src/fetch.ts) | runtime implementation | The source note names this file directly. | `named-file` |
| [`packages/web/tool-web/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/web/tool-web/src/index.ts) | package entry point | The source note names this file directly. Core file in the package named by the note: `packages/web/tool-web`. | `named-file, named-package-member` |
| [`packages/web/tool-web/src/search.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/web/tool-web/src/search.ts) | runtime implementation | The source note names this file directly. | `named-file` |
| [`packages/web/tool-web/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/web/tool-web/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/web/tool-web`. | `named-package-member` |
| [`packages/web/tool-web`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/packages/web/tool-web) | package or module directory | The note names this package or capability. | `named-package` |
| [`packages/web/web/src/types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/web/web/src/types.ts) | public types and contract | Defines `WebError`, a construct named by the note. | `symbol-definition` |
| [`packages/web/tool-web/package.json`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/web/tool-web/package.json) | composition and configuration | Core file in the package named by the note: `packages/web/tool-web`. | `named-package-member` |
| [`pnpm-lock.yaml`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/pnpm-lock.yaml) | composition and configuration | Contains the exact code literal `dsh-tool-web` named by the note. | `exact-code-occurrence` |
| [`docs/tool-catalog.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/tool-catalog.md) | package contract and examples | Contains the exact code literal `dsh-tool-web` named by the note. Contains the exact code literal `packages/web/tool-web/src/index.ts` named by the note. | `exact-code-occurrence` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `WebError` | `class` | [`packages/web/web/src/types.ts:129`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/web/web/src/types.ts#L129) | `export class WebError extends HarnessError {}` |

### Tests and executable evidence

- [`packages/web/web/tests/web.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/web/web/tests/web.spec.ts) — A test under the owning area exercises or imports `WebError`.
- [`packages/web/tool-web/tests/spill.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/web/tool-web/tests/spill.spec.ts) — A test under the owning area exercises or imports `dsh-tool-web`.
- [`packages/web/tool-web/tests/tool-web.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/web/tool-web/tests/tool-web.spec.ts) — A test under the owning area exercises or imports `dsh-tool-web`. A test under the owning area exercises or imports `WebError`.
- [`packages/web/tool-web/tests/load-path.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/web/tool-web/tests/load-path.spec.ts) — A test under the owning area exercises or imports `dsh-tool-web`.
- [`packages/web/tool-web/tests/integration.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/web/tool-web/tests/integration.spec.ts) — A test under the owning area exercises or imports `dsh-tool-web`.
- [`packages/core/tools/tests/tools.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/tests/tools.spec.ts) — Contains the exact code literal `tools/change` named by the note.
- [`packages/core/tools/tests/invariant.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/tests/invariant.spec.ts) — Contains the exact code literal `tools/change` named by the note.
- [`packages/typert/registry/tests/typert.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/typert/registry/tests/typert.spec.ts) — Contains the exact code literal `tools/change` named by the note.
- Source verification intent: No providers-change, searchStatus, fetchStatus, or WebCapabilityStatus spelling survives outside Agent Note history; the catalog is fresh (verify-cordis-catalog green); registration/disposal HMR-safety tests prove cleanup through execution behavior; and the tool-web README plus the architecture paragraph describe the execution-time error-routing contract the tool actually has.

## How to read the implementation

1. Start with [`docs/architecture.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/architecture.md) because it has the strongest evidence link to the note.
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

- Tags: `class/simplification`, `concern/boundary`, `concern/discovery-routing`, `concern/evidence`, `concern/ownership`, `concern/performance`, `concern/simplification`, `domain/extensions`, `domain/llm`, `domain/observability`, `domain/session-state`, `domain/testing`, `lifecycle/archived`, `mechanism/adapter`, `mechanism/projection`, `mechanism/registry`
- Aliases: `WebError`, `providers-change`, `WebService`, `web/providers-change`, `packages/web/web/src/index.ts`, `searchStatus`, `fetchStatus`, `WebCapabilityStatus`, `dsh-tool-web`, `ctx.web.search`, `packages/web/tool-web/src/search.ts`, `packages/web/tool-web/src/fetch.ts`, `packages/web/tool-web/README.md`, `packages/web/tool-web/src/index.ts`
- Regex: `(?i)(WebError|providers\-change|WebService|web/providers\-change|packages/web/web/src/index\.ts|searchStatus|fetchStatus|WebCapabilityStatus)`

```bash
rg -n --pcre2 "(?i)(WebError|providers\\-change|WebService|web/providers\\-change|packages/web/web/src/index\\.ts|searchStatus|fetchStatus|WebCapabilityStatus)" source-deepseek-harness
rg -l --fixed-strings "class/simplification" 03-agent-context
```

## Connected notes

- **`source-link`** — [0650. Drop the unconsumed `llm/adapter-change` event](0650-drop-the-unconsumed-llm-adapter-change-event.md): The source note links to this decision directly.
- **`shares-code-with`** — [0663. Prune unused web seam fields](0663-prune-unused-web-seam-fields.md): Shares source implementation: `packages/web/tool-web`, `packages/web/tool-web/README.md`.
- **`shares-code-with`** — [0672. Replace tool-web's regex HTML-to-markdown converter with turndown](0672-replace-tool-web-s-regex-html-to-markdown-converter-with-turndown.md): Shares source implementation: `packages/web/tool-web`, `packages/web/tool-web/README.md`.
- **`shares-code-with`** — [0235. Default Web search in shipped compositions](0235-default-web-search-in-shipped-compositions.md): Shares source implementation: `packages/web/tool-web/src/index.ts`, `packages/web/tool-web/src/invariant.ts`.
- **`shares-code-with`** — [0247. Web search source card scrolls instead of collapsing](0247-web-search-source-card-scrolls-instead-of-collapsing.md): Shares source implementation: `packages/web/tool-web/README.md`, `packages/web/tool-web/src/index.ts`.
- **`shares-code-with`** — [0018. Web capability seam - stable tools over multiple providers](0018-web-capability-seam-stable-tools-over-multiple-providers.md): Shares source implementation: `packages/web/web/src/index.ts`, `packages/web/web/src/types.ts`.
- **`shares-code-with`** — [0032. Tool output spill policy](0032-tool-output-spill-policy.md): Shares source implementation: `packages/web/tool-web/src/fetch.ts`, `packages/web/tool-web/src/index.ts`.
- **`shares-code-with`** — [0024. Prompt variables and tool-guidance ownership](0024-prompt-variables-and-tool-guidance-ownership.md): Shares source implementation: `packages/web/tool-web/src/fetch.ts`, `packages/web/tool-web/src/index.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0655-drop-the-unconsumed-web-observation-surface-the-providers-change-event-a.md`.
