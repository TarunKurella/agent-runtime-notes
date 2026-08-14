---
id: "dsh-note-0442"
title: "Documentation-site navigation and repository chrome"
status: "implemented"
class: "process"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/process/2026-08-12-documentation-site-navigation-and-chrome.md"
implementation_evidence: "high"
target_anchor: "repository tests and engineering policy"
tags:
  - "class/process"
  - "concern/boundary"
  - "concern/evidence"
  - "concern/ownership"
  - "concern/simplification"
  - "domain/build-release"
  - "domain/configuration"
  - "domain/extensions"
  - "domain/llm"
  - "domain/multi-agent"
  - "domain/session-state"
  - "domain/storage"
  - "domain/ui-interaction"
  - "domain/web-retrieval"
  - "lifecycle/implemented"
  - "mechanism/policy"
  - "mechanism/projection"
aliases:
  - "sections"
  - "indexOf"
  - "order"
  - "projectedPageContent"
  - "siteTitle"
  - "sectionSpec"
  - "orderedPages"
  - "landingLink"
  - "sectionOrder"
  - "-1"
  - "Array.prototype.sort"
  - "/guide/"
  - "guide/quickstart.md"
  - "SDK"
search_regex: "(?i)(sections|indexOf|order|projectedPageContent|siteTitle|sectionSpec|orderedPages|landingLink)"
---

# 0442. Documentation-site navigation and repository chrome — implementation context

## Open this when

The reference sidebar rendered its 43 subsystem pages first, ahead of every other group: sectionOrder in the VitePress config listed no position for the subsystem groups, nor for the group holding the Python SDK page, so indexOf returned -1 and sorted them ahead of the ordered sections. Clicking the 参考 navigation item landed on the architecture page whose own sidebar entry was link 44 of 62, 1549px down a 2478px sidebar --- outside the viewport.

## Source decision

website/docs.ts owns section placement. sections declares the groups per locale, and sectionSpec(locale, label) returns a group's position and collapse behavior, throwing when a locale declares no placement for a label. A group absent from the declaration now fails the build instead of sorting silently to the top. Placement is per locale because the two sidebars name their groups independently, and a label both use --- SDK --- cannot hold one rank against 入门 and against Guide at once.

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/process/2026-08-12-documentation-site-navigation-and-chrome.md](../02-notes/implemented/process/2026-08-12-documentation-site-navigation-and-chrome.md)
- Pinned source: [.agents/notes/implemented/process/2026-08-12-documentation-site-navigation-and-chrome.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/process/2026-08-12-documentation-site-navigation-and-chrome.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`website/docs.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/website/docs.ts) | runtime implementation | The source note names this file directly. Defines `sectionSpec`, a construct named by the note. | `exact-code-occurrence, named-file, symbol-definition` |
| [`scripts/project-doc-site.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/project-doc-site.ts) | repository automation | The source note names this file directly. Defines `projectedPageContent`, a construct named by the note. | `named-file, symbol-definition` |
| [`packages/client/ui-primitives/src/FishLogo.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-primitives/src/FishLogo.tsx) | runtime implementation | The source note names this file directly. | `named-file` |
| [`packages/subagent/subagent/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/subagent/subagent/src/index.ts) | package entry point | Core file in the package named by the note: `packages/subagent/subagent`. | `named-package-member` |
| [`packages/subagent/subagent/src/types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/subagent/subagent/src/types.ts) | public types and contract | Core file in the package named by the note: `packages/subagent/subagent`. | `named-package-member` |
| [`packages/subagent/subagent/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/subagent/subagent/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/subagent/subagent`. | `named-package-member` |
| [`packages/subagent/subagent`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/packages/subagent/subagent) | package or module directory | The note names this package or capability. | `named-package` |
| [`website/.vitepress/config.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/website/.vitepress/config.ts) | runtime implementation | Defines `siteTitle`, a construct named by the note. | `symbol-definition` |
| [`packages/skill/skill/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/skill/skill/src/index.ts) | package entry point | Defines `order`, a construct named by the note. | `symbol-definition` |
| [`packages/core/agent-loop/src/agent.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent-loop/src/agent.ts) | runtime implementation | Defines `sections`, a construct named by the note. | `symbol-definition` |
| [`packages/core/session/src/chunk-rows.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/chunk-rows.ts) | runtime implementation | Defines `indexOf`, a construct named by the note. | `symbol-definition` |
| [`packages/subagent/subagent/README.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/subagent/subagent/README.md) | package contract and examples | Core file in the package named by the note: `packages/subagent/subagent`. | `named-package-member` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `sections` | `const` | [`packages/core/agent-loop/src/agent.ts:232`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent-loop/src/agent.ts#L232) | `const sections = renderContextSections(assembly)` |
| `indexOf` | `function` | [`packages/core/session/src/chunk-rows.ts:131`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/chunk-rows.ts#L131) | `function indexOf(event: DeltaEvent): number {` |
| `order` | `const` | [`packages/skill/skill/src/index.ts:410`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/skill/skill/src/index.ts#L410) | `const order = this.nextProviderOrder` |
| `projectedPageContent` | `function` | [`scripts/project-doc-site.ts:333`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/project-doc-site.ts#L333) | `export function projectedPageContent(markdown: string, page: DocsPage): string {` |
| `siteTitle` | `function` | [`website/.vitepress/config.ts:245`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/website/.vitepress/config.ts#L245) | `function siteTitle(previewTag: string): string {` |
| `sectionSpec` | `function` | [`website/docs.ts:465`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/website/docs.ts#L465) | `export function sectionSpec(locale: DocsLocale, label: string): DocsSection & { index: number } {` |
| `orderedPages` | `function` | [`website/docs.ts:489`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/website/docs.ts#L489) | `export function orderedPages(locale: DocsLocale, collection: DocsSidebar): DocsPage[] {` |
| `landingLink` | `function` | [`website/docs.ts:520`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/website/docs.ts#L520) | `export function landingLink(locale: DocsLocale, collection: DocsSidebar): string {` |

### Tests and executable evidence

- [`scripts/project-doc-site.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/project-doc-site.spec.ts) — The source note names this file directly. Contains the exact code literal `guide/quickstart.md` named by the note.

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
- **`rust/process-discipline`** — Treat this as repository or release policy unless it changes a runtime contract. Do not add runtime machinery for a process-only lesson.
- **`rust/provider-adapter`** — Keep provider wire quirks inside adapters; the loop and durable event vocabulary remain provider-neutral.
- **`rust/quickjs-boundary`** — QuickJS holds orchestration variables; Rust/Tokio owns async execution and exposes only explicitly granted capabilities.
- **`rust/trait-seam`** — Use a small trait at the capability boundary; keep policy and model-facing schemas outside the backend implementation.

### Smallest safe implementation slice

1. Express the semantic contract as Rust types and one owning boundary.
2. Add the smallest adapter or service needed by the existing loop; do not add a new model-facing tool unless the four-tool surface cannot express the job.
3. Record durable evidence before deriving summaries or UI state.
4. Add one happy-path test, one interruption test, and one replay/restart test where state survives a turn.
5. Keep the patch reversible until the source note's strongest failure case is proven in the Rust harness.

## Search handles

- Tags: `class/process`, `concern/boundary`, `concern/evidence`, `concern/ownership`, `concern/simplification`, `domain/build-release`, `domain/configuration`, `domain/extensions`, `domain/llm`, `domain/multi-agent`, `domain/session-state`, `domain/storage`, `domain/ui-interaction`, `domain/web-retrieval`, `lifecycle/implemented`, `mechanism/policy`, `mechanism/projection`
- Aliases: `sections`, `indexOf`, `order`, `projectedPageContent`, `siteTitle`, `sectionSpec`, `orderedPages`, `landingLink`, `sectionOrder`, `-1`, `Array.prototype.sort`, `/guide/`, `guide/quickstart.md`, `SDK`
- Regex: `(?i)(sections|indexOf|order|projectedPageContent|siteTitle|sectionSpec|orderedPages|landingLink)`

```bash
rg -n --pcre2 "(?i)(sections|indexOf|order|projectedPageContent|siteTitle|sectionSpec|orderedPages|landingLink)" source-deepseek-harness
rg -l --fixed-strings "class/process" 03-agent-context
```

## Connected notes

- **`shares-code-with`** — [0285. Delegated subagents run with approvals pinned to `'never'`](0285-delegated-subagents-run-with-approvals-pinned-to-never.md): Shares source implementation: `packages/subagent/subagent`, `packages/subagent/subagent/src/index.ts`.
- **`shares-code-with`** — [0049. Canonical tool output contract](0049-canonical-tool-output-contract.md): Shares source implementation: `packages/skill/skill/src/index.ts`, `packages/subagent/subagent/src/index.ts`.
- **`shares-code-with`** — [0661. Trim unreachable ACP bridge surface --- the branding knobs and the kind-sniffing fallback](0661-trim-unreachable-acp-bridge-surface-the-branding-knobs-and-the-kind-snif.md): Shares source implementation: `packages/subagent/subagent`, `packages/subagent/subagent/src/index.ts`.
- **`shares-code-with`** — [0184. In-process subagent policy inheritance --- the child starts under the parent's sandbox override](0184-in-process-subagent-policy-inheritance-the-child-starts-under-the-parent.md): Shares source implementation: `packages/subagent/subagent`, `packages/subagent/subagent/src/index.ts`.
- **`shares-code-with`** — [0462. Merge subagent control into the subagent service](0462-merge-subagent-control-into-the-subagent-service.md): Shares source implementation: `packages/subagent/subagent`, `packages/subagent/subagent/src/index.ts`.
- **`shares-code-with`** — [0263. The continuable child return channel is an obligation](0263-the-continuable-child-return-channel-is-an-obligation.md): Shares source implementation: `packages/subagent/subagent`, `packages/subagent/subagent/src/index.ts`.
- **`shares-code-with`** — [0678. Record fork and mixed spawn+fork snapshot scenarios](0678-record-fork-and-mixed-spawn-fork-snapshot-scenarios.md): Shares source implementation: `packages/subagent/subagent`, `packages/subagent/subagent/src/index.ts`.
- **`shares-code-with`** — [0148. Background subagent tasks](0148-background-subagent-tasks.md): Shares source implementation: `packages/subagent/subagent`, `packages/subagent/subagent/src/index.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0442-documentation-site-navigation-and-repository-chrome.md`.
