---
id: "dsh-note-0674"
title: "Sidebar resize without a visible pill"
status: "archived"
class: "simplification"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/archived/simplification/2026-07-30-sidebar-resize-without-visible-pill.md"
implementation_evidence: "lead-only"
target_anchor: "optional UI or client layer"
tags:
  - "class/simplification"
  - "concern/boundary"
  - "concern/evidence"
  - "concern/lifecycle"
  - "concern/simplification"
  - "domain/testing"
  - "domain/ui-interaction"
  - "domain/web-retrieval"
  - "lifecycle/archived"
aliases:
  - "col-resize"
  - "Sidebar resize without a visible pill"
  - "simplification"
  - "boundary"
  - "evidence"
  - "lifecycle"
  - "testing"
  - "ui interaction"
  - "web retrieval"
  - "archived"
search_regex: "(?i)(col\\-resize|Sidebar[- ]resize[- ]without[- ]a[- ]visible[- ]pill|simplification|boundary|evidence|lifecycle|testing|ui[- ]interaction)"
---

# 0674. Sidebar resize without a visible pill — implementation context

## Open this when

The AppFrame exposed identical floating pills on both column borders. The left pill added unnecessary visual weight beside primary navigation, but the sidebar's resize interaction remains useful.

## Source decision

AppFrame keeps the sidebar's 8px resize hit strip, col-resize cursor, pointer capture, animation-frame throttling, and width updates, but does not generate the sidebar handle's pill pseudo-element. The details boundary retains both its hit strip and floating pill. The layout component test continues to pin sidebar dragging and both handles' collapse lifecycle. A keyless browser scenario reads the generated pseudo-elements from the shipped composition and drags the invisible sidebar boundary to prove the interaction remains live.

## Decision status

Historical design. Use it to understand an older pressure or failure; locate the replacing decision before implementing anything.

- Raw note: [archived/simplification/2026-07-30-sidebar-resize-without-visible-pill.md](../02-notes/archived/simplification/2026-07-30-sidebar-resize-without-visible-pill.md)
- Pinned source: [.agents/notes/archived/simplification/2026-07-30-sidebar-resize-without-visible-pill.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/archived/simplification/2026-07-30-sidebar-resize-without-visible-pill.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

No implementation path could be confirmed from the pinned tree. Use the search handles below; do not invent a source location.

### Code constructs named by the note

The note names behavior or policy but no uniquely resolvable code declaration. Search by its aliases and inspect the listed package entry points.

### Tests and executable evidence

- [`apps/web/tests/snapshots/details-session-lifecycle/handles.expected.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/snapshots/details-session-lifecycle/handles.expected.md) — A test under the owning area exercises or imports `col-resize`.

## How to read the implementation

1. Start from the source note and run the regex below across the pinned repository.
2. Read public types and entry points before implementation details. Write down the contract in plain language.
3. Follow the named constructs into their callers and tests. Confirm which component can actually enforce the rule.
4. Reconstruct one failure path: cancellation, crash, retry, stale state, partial write, or incompatible provider input—whichever the note is protecting against.
5. Only then map the contract into the current Rust repository. Target paths must come from that repository, never from guesswork in this wiki.

## Rust harness port

**Likely integration point:** optional UI or client layer.

Preserve the behavior and ownership rule. Do not mechanically translate Cordis, TypeScript declaration merging, Node streams, or package topology into Rust.

Relevant standing rules:

- **`rust/four-tool-surface`** — Preserve the four model tools: read, write, patch, and exec. Reuse internal capabilities without automatically exposing new model-facing tools.
- **`rust/trait-seam`** — Use a small trait at the capability boundary; keep policy and model-facing schemas outside the backend implementation.
- **Status gate** — Historical design. Use it to understand an older pressure or failure; locate the replacing decision before implementing anything.

### Smallest safe implementation slice

1. Express the semantic contract as Rust types and one owning boundary.
2. Add the smallest adapter or service needed by the existing loop; do not add a new model-facing tool unless the four-tool surface cannot express the job.
3. Record durable evidence before deriving summaries or UI state.
4. Add one happy-path test, one interruption test, and one replay/restart test where state survives a turn.
5. Keep the patch reversible until the source note's strongest failure case is proven in the Rust harness.

## Search handles

- Tags: `class/simplification`, `concern/boundary`, `concern/evidence`, `concern/lifecycle`, `concern/simplification`, `domain/testing`, `domain/ui-interaction`, `domain/web-retrieval`, `lifecycle/archived`
- Aliases: `col-resize`, `Sidebar resize without a visible pill`, `simplification`, `boundary`, `evidence`, `lifecycle`, `testing`, `ui interaction`, `web retrieval`, `archived`
- Regex: `(?i)(col\-resize|Sidebar[- ]resize[- ]without[- ]a[- ]visible[- ]pill|simplification|boundary|evidence|lifecycle|testing|ui[- ]interaction)`

```bash
rg -n --pcre2 "(?i)(col\\-resize|Sidebar[- ]resize[- ]without[- ]a[- ]visible[- ]pill|simplification|boundary|evidence|lifecycle|testing|ui[- ]interaction)" source-deepseek-harness
rg -l --fixed-strings "class/simplification" 03-agent-context
```

## Connected notes

- **`same-design-pressure`** — [0297. Workspace Sidebar Order and Folding](0297-workspace-sidebar-order-and-folding.md): Shares design concerns: `concern/boundary`, `concern/evidence`, `concern/simplification`.
- **`same-design-pressure`** — [0598. Address pending queue occurrences for edit and removal](0598-address-pending-queue-occurrences-for-edit-and-removal.md): Shares design concerns: `concern/boundary`, `concern/evidence`, `concern/lifecycle`.
- **`same-design-pressure`** — [0583. Docked web goal bar](0583-docked-web-goal-bar.md): Shares design concerns: `concern/boundary`, `concern/evidence`, `concern/lifecycle`.
- **`same-design-pressure`** — [0564. Bash-backed grep and glob discovery tools](0564-bash-backed-grep-and-glob-discovery-tools.md): Shares design concerns: `concern/boundary`, `concern/evidence`, `concern/lifecycle`.
- **`same-design-pressure`** — [0542. Dependency swaps rejected by the 2026-07 NIH audit](0542-dependency-swaps-rejected-by-the-2026-07-nih-audit.md): Shares design concerns: `concern/boundary`, `concern/evidence`, `concern/lifecycle`.
- **`same-design-pressure`** — [0527. Artifact-first NPM baseline publication](0527-artifact-first-npm-baseline-publication.md): Shares design concerns: `concern/boundary`, `concern/evidence`, `concern/lifecycle`.
- **`same-design-pressure`** — [0520. Task Surface for structured session interaction](0520-task-surface-for-structured-session-interaction.md): Shares design concerns: `concern/boundary`, `concern/evidence`, `concern/lifecycle`.
- **`same-design-pressure`** — [0514. Cordis Host/Client Dynamic Plugin Runtime](0514-cordis-host-client-dynamic-plugin-runtime.md): Shares design concerns: `concern/boundary`, `concern/evidence`, `concern/lifecycle`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0674-sidebar-resize-without-a-visible-pill.md`.
