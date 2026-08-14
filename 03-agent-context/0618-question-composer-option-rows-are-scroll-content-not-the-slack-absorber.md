---
id: "dsh-note-0618"
title: "Question-composer option rows are scroll content, not the slack absorber"
status: "archived"
class: "bug-fix"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/archived/bug-fix/2026-07-27-question-composer-rows-do-not-shrink.md"
implementation_evidence: "lead-only"
target_anchor: "ContextFrame compiler and evidence-preserving compaction"
tags:
  - "class/bug-fix"
  - "concern/evidence"
  - "concern/lifecycle"
  - "concern/ownership"
  - "concern/simplification"
  - "domain/build-release"
  - "domain/context"
  - "domain/session-state"
  - "domain/testing"
  - "domain/ui-interaction"
  - "domain/web-retrieval"
  - "lifecycle/archived"
  - "mechanism/event-sourcing"
aliases:
  - "max-height: min"
  - ".options"
  - "flex-direction: column"
  - "flex-shrink: 1"
  - "min-height: 42px"
  - ".optionCopy"
  - "align-items: center"
  - "scrollHeight === clientHeight"
  - ".option"
  - ".custom"
  - "flex-shrink: 0"
  - "overflow-y: auto"
  - "min-height: 0"
  - ".header"
search_regex: "(?i)(max\\-height:[- ]min|\\.options|flex\\-direction:[- ]column|flex\\-shrink:[- ]1|min\\-height:[- ]42px|\\.optionCopy|align\\-items:[- ]center|scrollHeight[- ]===[- ]clientHeight)"
---

# 0618. Question-composer option rows are scroll content, not the slack absorber — implementation context

## Open this when

The question composer card is capped against the viewport (max-height: min(60vh, 520px)) and scrolls its option list, so the header and the footer actions stay reachable on long question batches. When the composer seat got short --- a small window, or a short viewport with the details panel open --- the option rows rendered on top of each other and on top of the question title. The cap was not the defect; the distribution of the shortfall was. .options is a flex-direction: column box whose children default to flex-shrink: 1, so under-allocation shrank the rows first instead of overflowing the scroll container.

## Source decision

.option and .custom declare flex-shrink: 0. The rows are the scroll content of a capped card; the card's overflow belongs to .options, which already owns overflow-y: auto and min-height: 0. Pinning the children makes the shortfall reach that scroll container instead of being absorbed by the rows, which is the behavior the cap was designed for. The alternative --- letting rows shrink but keeping the copy inside them --- would require clipping or ellipsizing option descriptions at exactly the sizes where the user most needs to read them.

## Decision status

Historical design. Use it to understand an older pressure or failure; locate the replacing decision before implementing anything.

- Raw note: [archived/bug-fix/2026-07-27-question-composer-rows-do-not-shrink.md](../02-notes/archived/bug-fix/2026-07-27-question-composer-rows-do-not-shrink.md)
- Pinned source: [.agents/notes/archived/bug-fix/2026-07-27-question-composer-rows-do-not-shrink.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/archived/bug-fix/2026-07-27-question-composer-rows-do-not-shrink.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

No implementation path could be confirmed from the pinned tree. Use the search handles below; do not invent a source location.

### Code constructs named by the note

The note names behavior or policy but no uniquely resolvable code declaration. Search by its aliases and inspect the listed package entry points.

### Tests and executable evidence

- [`scripts/client-bundle-css.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/client-bundle-css.spec.ts) — A test under the owning area exercises or imports `css`.
- [`scripts/publication-payload.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/publication-payload.spec.ts) — A test under the owning area exercises or imports `css`.
- [`apps/web/tests/seeded-history.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/seeded-history.e2e.ts) — A test under the owning area exercises or imports `scrollHeight`. A test under the owning area exercises or imports `clientHeight`.
- [`scripts/client-bundle-purity.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/client-bundle-purity.spec.ts) — A test under the owning area exercises or imports `css`.
- [`apps/web/tests/produced-files.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/produced-files.e2e.ts) — A test under the owning area exercises or imports `css`.
- [`apps/web/tests/web-search-round.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/web-search-round.e2e.ts) — A test under the owning area exercises or imports `max-height`. A test under the owning area exercises or imports `scrollHeight`.
- [`apps/web/tests/question-composer.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/question-composer.e2e.ts) — A test under the owning area exercises or imports `Blue`. A test under the owning area exercises or imports `Green`.
- [`apps/web/tests/sidebar-scrollbar.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/sidebar-scrollbar.e2e.ts) — A test under the owning area exercises or imports `scrollHeight`. A test under the owning area exercises or imports `clientHeight`.
- Source verification intent: The web e2e composer scenario asserts the invariant on the live composer at three squeezed seat heights (900x520 / 440 / 380): every option row's children stay inside the row's border box. Two guards keep the assertion from holding vacuously --- at least one row must be wrapped (the only shape that overflows) and .options must actually be scrolling (proof the seat is genuinely capped). The scenario's recorded question now carries long option descriptions for exactly that reason; without wrapping copy the assertion cannot fail.

## How to read the implementation

1. Start from the source note and run the regex below across the pinned repository.
2. Read public types and entry points before implementation details. Write down the contract in plain language.
3. Follow the named constructs into their callers and tests. Confirm which component can actually enforce the rule.
4. Reconstruct one failure path: cancellation, crash, retry, stale state, partial write, or incompatible provider input—whichever the note is protecting against.
5. Only then map the contract into the current Rust repository. Target paths must come from that repository, never from guesswork in this wiki.

## Rust harness port

**Likely integration point:** ContextFrame compiler and evidence-preserving compaction.

Preserve the behavior and ownership rule. Do not mechanically translate Cordis, TypeScript declaration merging, Node streams, or package topology into Rust.

Relevant standing rules:

- **`rust/append-log`** — Keep durable facts append-only and derive views from them. Separate the synchronous fact boundary from asynchronous persistence.
- **`rust/context-evidence`** — Compaction may shrink presentation, but it must not erase the evidence ledger, file activity, or durable session facts.
- **`rust/four-tool-surface`** — Preserve the four model tools: read, write, patch, and exec. Reuse internal capabilities without automatically exposing new model-facing tools.
- **`rust/process-discipline`** — Treat this as repository or release policy unless it changes a runtime contract. Do not add runtime machinery for a process-only lesson.
- **Status gate** — Historical design. Use it to understand an older pressure or failure; locate the replacing decision before implementing anything.

### Smallest safe implementation slice

1. Express the semantic contract as Rust types and one owning boundary.
2. Add the smallest adapter or service needed by the existing loop; do not add a new model-facing tool unless the four-tool surface cannot express the job.
3. Record durable evidence before deriving summaries or UI state.
4. Add one happy-path test, one interruption test, and one replay/restart test where state survives a turn.
5. Keep the patch reversible until the source note's strongest failure case is proven in the Rust harness.

## Search handles

- Tags: `class/bug-fix`, `concern/evidence`, `concern/lifecycle`, `concern/ownership`, `concern/simplification`, `domain/build-release`, `domain/context`, `domain/session-state`, `domain/testing`, `domain/ui-interaction`, `domain/web-retrieval`, `lifecycle/archived`, `mechanism/event-sourcing`
- Aliases: `max-height: min`, `.options`, `flex-direction: column`, `flex-shrink: 1`, `min-height: 42px`, `.optionCopy`, `align-items: center`, `scrollHeight === clientHeight`, `.option`, `.custom`, `flex-shrink: 0`, `overflow-y: auto`, `min-height: 0`, `.header`
- Regex: `(?i)(max\-height:[- ]min|\.options|flex\-direction:[- ]column|flex\-shrink:[- ]1|min\-height:[- ]42px|\.optionCopy|align\-items:[- ]center|scrollHeight[- ]===[- ]clientHeight)`

```bash
rg -n --pcre2 "(?i)(max\\-height:[- ]min|\\.options|flex\\-direction:[- ]column|flex\\-shrink:[- ]1|min\\-height:[- ]42px|\\.optionCopy|align\\-items:[- ]center|scrollHeight[- ]===[- ]clientHeight)" source-deepseek-harness
rg -l --fixed-strings "class/bug-fix" 03-agent-context
```

## Connected notes

- **`shares-code-with`** — [0351. Reader scroll attribution through the observed-top ledger](0351-reader-scroll-attribution-through-the-observed-top-ledger.md): Shares source implementation: `apps/web/tests/approval-composer.e2e.ts`, `apps/web/tests/chat-scroll-contract.e2e.ts`.
- **`shares-code-with`** — [0248. Web turn run time and hover-revealed time chrome](0248-web-turn-run-time-and-hover-revealed-time-chrome.md): Shares source implementation: `apps/web/tests/produced-files.e2e.ts`, `apps/web/tests/sidebar-scrollbar.e2e.ts`.
- **`shares-code-with`** — [0282. Creator guidance lands as an introduce cue on the preset chip](0282-creator-guidance-lands-as-an-introduce-cue-on-the-preset-chip.md): Shares source implementation: `apps/web/tests/approval-composer.e2e.ts`, `apps/web/tests/chat-scroll-contract.e2e.ts`.
- **`shares-code-with`** — [0247. Web search source card scrolls instead of collapsing](0247-web-search-source-card-scrolls-instead-of-collapsing.md): Shares source implementation: `apps/web/tests/web-search-round.e2e.ts`.
- **`shares-code-with`** — [0628. Web favicon follows the color scheme](0628-web-favicon-follows-the-color-scheme.md): Shares source implementation: `apps/web/tests/produced-files.e2e.ts`, `apps/web/tests/seeded-history.e2e.ts`.
- **`shares-code-with`** — [0327. The composer's two text layers share one scrollport](0327-the-composer-s-two-text-layers-share-one-scrollport.md): Shares source implementation: `scripts/client-bundle-purity.spec.ts`.
- **`shares-code-with`** — [0320. The approval takeover shares the composer's text cap](0320-the-approval-takeover-shares-the-composer-s-text-cap.md): Shares source implementation: `apps/web/tests/approval-composer.e2e.ts`.
- **`shares-code-with`** — [0296. Status-driven disclosure for workflow runs](0296-status-driven-disclosure-for-workflow-runs.md): Shares source implementation: `apps/web/tests/seeded-history.e2e.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0618-question-composer-option-rows-are-scroll-content-not-the-slack-absorber.md`.
