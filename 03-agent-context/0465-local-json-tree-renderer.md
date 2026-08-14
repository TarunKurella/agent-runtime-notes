---
id: "dsh-note-0465"
title: "Local JSON tree renderer"
status: "implemented"
class: "simplification"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/simplification/2026-07-28-local-json-tree-renderer.md"
implementation_evidence: "medium"
target_anchor: "append-only event and session store"
tags:
  - "class/simplification"
  - "concern/boundary"
  - "concern/evidence"
  - "concern/lifecycle"
  - "concern/ownership"
  - "concern/recovery"
  - "concern/simplification"
  - "domain/session-state"
  - "domain/testing"
  - "domain/ui-interaction"
  - "lifecycle/implemented"
aliases:
  - "JsonTree"
  - "react-json-view-lite"
  - "dsh-client-ui-primitives"
  - "expandTopLevel"
  - "Local JSON tree renderer"
  - "simplification"
  - "boundary"
  - "evidence"
  - "lifecycle"
  - "ownership"
  - "recovery"
  - "session state"
  - "testing"
  - "ui interaction"
search_regex: "(?i)(JsonTree|react\\-json\\-view\\-lite|dsh\\-client\\-ui\\-primitives|expandTopLevel|Local[- ]JSON[- ]tree[- ]renderer|simplification|boundary|evidence)"
---

# 0465. Local JSON tree renderer — implementation context

## Open this when

The read-only JSON inspector used by the trajectory ledger needs compact object and array previews, explicit array paths for copy actions, fixed-open and collapsible root modes, and keyboard navigation. react-json-view-lite exposes neither custom node rendering nor row identity, so satisfying those requirements through that dependency requires a package-manager patch against compiled distribution files and DOM traversal that reconstructs data paths from visible labels. The patch behaves as an untyped fork while its source maps and upstream source remain unchanged.

## Source decision

JsonTree owns its recursive presentation in dsh-client-ui-primitives. Each rendered row receives its value and property path directly. Object keys and array indexes extend that path during recursion, so copy actions never recover application data from rendered DOM text. Expandable rows render the compact preview locally and mount child rows only while expanded. expandTopLevel selects between a fixed-open bracket frame and a collapsible root node without changing the public component contract. The tree keeps one tabbable expander among visible nodes.

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/simplification/2026-07-28-local-json-tree-renderer.md](../02-notes/implemented/simplification/2026-07-28-local-json-tree-renderer.md)
- Pinned source: [.agents/notes/implemented/simplification/2026-07-28-local-json-tree-renderer.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/simplification/2026-07-28-local-json-tree-renderer.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`packages/client/ui-primitives/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-primitives/src/index.ts) | package entry point | Core file in the package named by the note: `packages/client/ui-primitives`. | `named-package-member` |
| [`packages/client/ui-primitives/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-primitives/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/client/ui-primitives`. | `named-package-member` |
| [`packages/client/ui-primitives/src/JsonTree.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-primitives/src/JsonTree.tsx) | runtime implementation | Core file in the package named by the note: `packages/client/ui-primitives`. Defines `JsonTree`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/client/ui-primitives`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-primitives) | package or module directory | The note names this package or capability. | `named-package` |
| [`packages/client/ui-primitives/README.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-primitives/README.md) | package contract and examples | Core file in the package named by the note: `packages/client/ui-primitives`. | `named-package-member` |
| [`packages/client/ui-primitives/package.json`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-primitives/package.json) | composition and configuration | Core file in the package named by the note: `packages/client/ui-primitives`. | `named-package-member` |
| [`knip.json`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/knip.json) | composition and configuration | Contains the exact code literal `dsh-client-ui-primitives` named by the note. | `exact-code-occurrence` |
| [`pnpm-lock.yaml`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/pnpm-lock.yaml) | composition and configuration | Contains the exact code literal `dsh-client-ui-primitives` named by the note. | `exact-code-occurrence` |
| [`tsconfig.base.json`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/tsconfig.base.json) | composition and configuration | Contains the exact code literal `dsh-client-ui-primitives` named by the note. | `exact-code-occurrence` |
| [`apps/web/package.json`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/package.json) | composition and configuration | Contains the exact code literal `dsh-client-ui-primitives` named by the note. | `exact-code-occurrence` |
| [`docs/config-catalog.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/config-catalog.md) | package contract and examples | Contains the exact code literal `dsh-client-ui-primitives` named by the note. | `exact-code-occurrence` |
| [`docs/config-catalog.zh.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/config-catalog.zh.md) | package contract and examples | Contains the exact code literal `dsh-client-ui-primitives` named by the note. | `exact-code-occurrence` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `JsonTree` | `function` | [`packages/client/ui-primitives/src/JsonTree.tsx:407`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-primitives/src/JsonTree.tsx#L407) | `export function JsonTree({` |

### Tests and executable evidence

- [`packages/client/ui-primitives/tests/json-tree.client.spec.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-primitives/tests/json-tree.client.spec.tsx) — A test under the owning area exercises or imports `JsonTree`. A test under the owning area exercises or imports `expandTopLevel`.
- [`scripts/client-bundle-purity.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/client-bundle-purity.spec.ts) — Contains the exact code literal `dsh-client-ui-primitives` named by the note.

## How to read the implementation

1. Start with [`packages/client/ui-primitives/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-primitives/src/index.ts) because it has the strongest evidence link to the note.
2. Read public types and entry points before implementation details. Write down the contract in plain language.
3. Follow the named constructs into their callers and tests. Confirm which component can actually enforce the rule.
4. Reconstruct one failure path: cancellation, crash, retry, stale state, partial write, or incompatible provider input—whichever the note is protecting against.
5. Only then map the contract into the current Rust repository. Target paths must come from that repository, never from guesswork in this wiki.

## Rust harness port

**Likely integration point:** append-only event and session store.

Preserve the behavior and ownership rule. Do not mechanically translate Cordis, TypeScript declaration merging, Node streams, or package topology into Rust.

Relevant standing rules:

- **`rust/append-log`** — Keep durable facts append-only and derive views from them. Separate the synchronous fact boundary from asynchronous persistence.
- **`rust/four-tool-surface`** — Preserve the four model tools: read, write, patch, and exec. Reuse internal capabilities without automatically exposing new model-facing tools.
- **`rust/trait-seam`** — Use a small trait at the capability boundary; keep policy and model-facing schemas outside the backend implementation.

### Smallest safe implementation slice

1. Express the semantic contract as Rust types and one owning boundary.
2. Add the smallest adapter or service needed by the existing loop; do not add a new model-facing tool unless the four-tool surface cannot express the job.
3. Record durable evidence before deriving summaries or UI state.
4. Add one happy-path test, one interruption test, and one replay/restart test where state survives a turn.
5. Keep the patch reversible until the source note's strongest failure case is proven in the Rust harness.

## Search handles

- Tags: `class/simplification`, `concern/boundary`, `concern/evidence`, `concern/lifecycle`, `concern/ownership`, `concern/recovery`, `concern/simplification`, `domain/session-state`, `domain/testing`, `domain/ui-interaction`, `lifecycle/implemented`
- Aliases: `JsonTree`, `react-json-view-lite`, `dsh-client-ui-primitives`, `expandTopLevel`, `Local JSON tree renderer`, `simplification`, `boundary`, `evidence`, `lifecycle`, `ownership`, `recovery`, `session state`, `testing`, `ui interaction`
- Regex: `(?i)(JsonTree|react\-json\-view\-lite|dsh\-client\-ui\-primitives|expandTopLevel|Local[- ]JSON[- ]tree[- ]renderer|simplification|boundary|evidence)`

```bash
rg -n --pcre2 "(?i)(JsonTree|react\\-json\\-view\\-lite|dsh\\-client\\-ui\\-primitives|expandTopLevel|Local[- ]JSON[- ]tree[- ]renderer|simplification|boundary|evidence)" source-deepseek-harness
rg -l --fixed-strings "class/simplification" 03-agent-context
```

## Connected notes

- **`source-link`** — [0194. Trajectory inspection ledger](0194-trajectory-inspection-ledger.md): The source note links to this decision directly.
- **`shares-code-with`** — [0412. Web client syntax highlighting --- synchronous fine-grained shiki](0412-web-client-syntax-highlighting-synchronous-fine-grained-shiki.md): Shares source implementation: `packages/client/ui-primitives`, `packages/client/ui-primitives/README.md`.
- **`shares-code-with`** — [0520. Task Surface for structured session interaction](0520-task-surface-for-structured-session-interaction.md): Shares source implementation: `packages/client/ui-primitives/src/index.ts`, `packages/client/ui-primitives/src/invariant.ts`.
- **`shares-code-with`** — [0676. Explicit-config dsh entrypoint](0676-explicit-config-dsh-entrypoint.md): Shares source implementation: `knip.json`, `pnpm-lock.yaml`.
- **`shares-code-with`** — [0225. Web search card --- the grep and glob render intent reaches the browser](0225-web-search-card-the-grep-and-glob-render-intent-reaches-the-browser.md): Shares source implementation: `packages/client/ui-primitives/src/index.ts`, `packages/client/ui-primitives/src/invariant.ts`.
- **`shares-code-with`** — [0223. Web result card frontend --- rendering the web render intent in the browser](0223-web-result-card-frontend-rendering-the-web-render-intent-in-the-browser.md): Shares source implementation: `packages/client/ui-primitives/src/index.ts`, `packages/client/ui-primitives/src/invariant.ts`.
- **`shares-code-with`** — [0220. Web read card frontend --- the read tool's line window renders line-numbered and highlighted](0220-web-read-card-frontend-the-read-tool-s-line-window-renders-line-numbered.md): Shares source implementation: `packages/client/ui-primitives/src/index.ts`, `packages/client/ui-primitives/src/invariant.ts`.
- **`shares-code-with`** — [0294. Web attachment display aligns with DeepSeek Chat via attachment atoms](0294-web-attachment-display-aligns-with-deepseek-chat-via-attachment-atoms.md): Shares source implementation: `packages/client/ui-primitives/src/index.ts`, `packages/client/ui-primitives/src/invariant.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0465-local-json-tree-renderer.md`.
