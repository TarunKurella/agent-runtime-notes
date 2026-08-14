---
id: "dsh-note-0639"
title: "Generate the Cordis core API reference"
status: "archived"
class: "process"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/archived/process/2026-07-20-generated-cordis-core-api.md"
implementation_evidence: "high"
target_anchor: "repository tests and engineering policy"
tags:
  - "class/process"
  - "concern/boundary"
  - "concern/discovery-routing"
  - "concern/evidence"
  - "concern/ownership"
  - "concern/recovery"
  - "domain/build-release"
  - "domain/extensions"
  - "domain/ui-interaction"
  - "lifecycle/archived"
  - "mechanism/policy"
  - "mechanism/registry"
aliases:
  - "ctx"
  - "scripts/cordis-core-api.ts"
  - "vendor/cordis/src"
  - "docs/cordis-catalog/core/"
  - "scripts/gen-cordis-catalog.ts"
  - "verify-cordis-catalog"
  - "ts cordis-catalog"
  - "ctx.*"
  - "website/docs.ts"
  - "/reference/cordis-api/"
  - "/en/reference/cordis-api/"
  - "Generate the Cordis core API reference"
  - "process"
  - "boundary"
search_regex: "(?i)(scripts/cordis\\-core\\-api\\.ts|vendor/cordis/src|docs/cordis\\-catalog/core/|scripts/gen\\-cordis\\-catalog\\.ts|verify\\-cordis\\-catalog|ts[- ]cordis\\-catalog|ctx\\.\\*|website/docs\\.ts)"
---

# 0639. Generate the Cordis core API reference — implementation context

## Open this when

Plugin authors need the detailed Cordis APIs behind ctx, event dispatch, fibers, plugin registration, and services. The generated Harness event and service catalogs intentionally summarize inherited Cordis members, so they do not replace a method-level Cordis reference. Keeping a second hand-written copy under the website would drift from the vendored source and make the renderer an additional documentation owner.

## Source decision

scripts/cordis-core-api.ts reads the public declarations and original JSDoc from vendor/cordis/src with the TypeScript compiler API. An explicit page manifest generates five files under docs/cordis-catalog/core/: Context, Events, Fiber, Registry, and Service. scripts/gen-cordis-catalog.ts writes these pages together with the Harness event and service catalogs, and verify-cordis-catalog rejects stale output. The generator validates that documented classes and methods retain descriptive JSDoc, including parameter and non-void return contracts.

## Decision status

Historical design. Use it to understand an older pressure or failure; locate the replacing decision before implementing anything.

- Raw note: [archived/process/2026-07-20-generated-cordis-core-api.md](../02-notes/archived/process/2026-07-20-generated-cordis-core-api.md)
- Pinned source: [.agents/notes/archived/process/2026-07-20-generated-cordis-core-api.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/archived/process/2026-07-20-generated-cordis-core-api.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`scripts/cordis-core-api.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/cordis-core-api.ts) | repository automation | The source note names this file directly. Contains the exact code literal `scripts/gen-cordis-catalog.ts` named by the note. | `exact-code-occurrence, named-file` |
| [`scripts/gen-cordis-catalog.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/gen-cordis-catalog.ts) | repository automation | The source note names this file directly. | `named-file` |
| [`vendor/cordis/src`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/vendor/cordis/src) | package or module directory | The source note names this implementation area directly. | `named-directory` |
| [`apps/cli/src/profile-boot.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/cli/src/profile-boot.ts) | runtime implementation | Defines `ctx`, a construct named by the note. | `symbol-definition` |
| [`packages/core/tools/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/index.ts) | package entry point | Defines `ctx`, a construct named by the note. | `symbol-definition` |
| [`packages/client/web/src/boot.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/web/src/boot.tsx) | runtime implementation | Defines `ctx`, a construct named by the note. | `symbol-definition` |
| [`packages/boot/app-boot/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/boot/app-boot/src/index.ts) | package entry point | Defines `ctx`, a construct named by the note. | `symbol-definition` |
| [`package.json`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/package.json) | composition and configuration | Contains the exact code literal `scripts/gen-cordis-catalog.ts` named by the note. | `exact-code-occurrence` |
| [`tsconfig.base.json`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/tsconfig.base.json) | composition and configuration | Contains the exact code literal `vendor/cordis/src` named by the note. | `exact-code-occurrence` |
| [`docs/cordis-api/events.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/cordis-api/events.md) | package contract and examples | Contains the exact code literal `scripts/gen-cordis-catalog.ts` named by the note. | `exact-code-occurrence` |
| [`docs/cordis-api/service.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/cordis-api/service.md) | package contract and examples | Contains the exact code literal `scripts/gen-cordis-catalog.ts` named by the note. | `exact-code-occurrence` |
| [`docs/cordis-api/fiber.zh.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/cordis-api/fiber.zh.md) | package contract and examples | Contains the exact code literal `scripts/gen-cordis-catalog.ts` named by the note. | `exact-code-occurrence` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `ctx` | `const` | [`apps/cli/src/profile-boot.ts:248`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/cli/src/profile-boot.ts#L248) | `const ctx = await boot(NAME, rootConfig, structuredClone(allPatches(composed)), (hostCtx) => {` |
| `ctx` | `const` | [`packages/boot/app-boot/src/index.ts:764`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/boot/app-boot/src/index.ts#L764) | `const ctx = new Context()` |
| `ctx` | `const` | [`packages/client/web/src/boot.tsx:162`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/web/src/boot.tsx#L162) | `const ctx = this.ctx` |
| `ctx` | `const` | [`packages/client/web/src/boot.tsx:217`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/web/src/boot.tsx#L217) | `const ctx = this.ctx` |
| `ctx` | `const` | [`packages/core/tools/src/index.ts:947`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/index.ts#L947) | `const ctx = this.ctx` |

### Tests and executable evidence

- [`scripts/cordis-core-api.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/cordis-core-api.spec.ts) — Contains the exact code literal `vendor/cordis/src` named by the note.
- [`scripts/project-doc-site.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/project-doc-site.spec.ts) — Contains the exact code literal `website/docs.ts` named by the note.

## How to read the implementation

1. Start with [`scripts/cordis-core-api.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/cordis-core-api.ts) because it has the strongest evidence link to the note.
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

- Tags: `class/process`, `concern/boundary`, `concern/discovery-routing`, `concern/evidence`, `concern/ownership`, `concern/recovery`, `domain/build-release`, `domain/extensions`, `domain/ui-interaction`, `lifecycle/archived`, `mechanism/policy`, `mechanism/registry`
- Aliases: `ctx`, `scripts/cordis-core-api.ts`, `vendor/cordis/src`, `docs/cordis-catalog/core/`, `scripts/gen-cordis-catalog.ts`, `verify-cordis-catalog`, `ts cordis-catalog`, `ctx.*`, `website/docs.ts`, `/reference/cordis-api/`, `/en/reference/cordis-api/`, `Generate the Cordis core API reference`, `process`, `boundary`
- Regex: `(?i)(scripts/cordis\-core\-api\.ts|vendor/cordis/src|docs/cordis\-catalog/core/|scripts/gen\-cordis\-catalog\.ts|verify\-cordis\-catalog|ts[- ]cordis\-catalog|ctx\.\*|website/docs\.ts)`

```bash
rg -n --pcre2 "(?i)(scripts/cordis\\-core\\-api\\.ts|vendor/cordis/src|docs/cordis\\-catalog/core/|scripts/gen\\-cordis\\-catalog\\.ts|verify\\-cordis\\-catalog|ts[- ]cordis\\-catalog|ctx\\.\\*|website/docs\\.ts)" source-deepseek-harness
rg -l --fixed-strings "class/process" 03-agent-context
```

## Connected notes

- **`source-link`** — [0632. Generated cordis events + services catalog](0632-generated-cordis-events-services-catalog.md): The source note links to this decision directly.
- **`shares-code-with`** — [0554. dsh-tui chat channel module split](0554-dsh-tui-chat-channel-module-split.md): Shares source implementation: `apps/cli/src/profile-boot.ts`, `packages/boot/app-boot/src/index.ts`.
- **`shares-code-with`** — [0196. Web session fork actions](0196-web-session-fork-actions.md): Shares source implementation: `apps/cli/src/profile-boot.ts`, `packages/client/web/src/boot.tsx`.
- **`shares-code-with`** — [0416. Per-subsystem generated cordis-surface regions](0416-per-subsystem-generated-cordis-surface-regions.md): Shares source implementation: `apps/cli/src/profile-boot.ts`, `scripts/gen-cordis-catalog.ts`.
- **`shares-code-with`** — [0329. fail-loud releases the terminal before exiting](0329-fail-loud-releases-the-terminal-before-exiting.md): Shares source implementation: `apps/cli/src/profile-boot.ts`, `packages/boot/app-boot/src/index.ts`.
- **`shares-code-with`** — [0137. The `todo_write` tool --- model task list as event-sourced session state](0137-the-todo-write-tool-model-task-list-as-event-sourced-session-state.md): Shares source implementation: `packages/core/tools/src/index.ts`, `scripts/gen-cordis-catalog.ts`.
- **`shares-code-with`** — [0659. Remove the `agent/steering` mirror emit](0659-remove-the-agent-steering-mirror-emit.md): Shares source implementation: `apps/cli/src/profile-boot.ts`, `packages/client/web/src/boot.tsx`.
- **`shares-code-with`** — [0145. Explicit model-facing tool order](0145-explicit-model-facing-tool-order.md): Shares source implementation: `packages/boot/app-boot/src/index.ts`, `packages/core/tools/src/index.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0639-generate-the-cordis-core-api-reference.md`.
