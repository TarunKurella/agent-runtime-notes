---
id: "dsh-note-0261"
title: "Web preview product badge"
status: "implemented"
class: "feature"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/feature/2026-08-05-web-preview-product-badge.md"
implementation_evidence: "lead-only"
target_anchor: "append-only event and session store"
tags:
  - "class/feature"
  - "concern/evidence"
  - "concern/human-control"
  - "concern/lifecycle"
  - "domain/build-release"
  - "domain/session-state"
  - "domain/testing"
  - "lifecycle/implemented"
aliases:
  - "Web preview product badge"
  - "feature"
  - "evidence"
  - "human control"
  - "lifecycle"
  - "build release"
  - "session state"
  - "testing"
  - "implemented"
search_regex: "(?i)(Web[- ]preview[- ]product[- ]badge|feature|evidence|human[- ]control|lifecycle|build[- ]release|session[- ]state|testing)"
---

# 0261. Web preview product badge — implementation context

## Open this when

The Web empty state does not identify the product as a preview. Users can enter the main session surface without seeing that the product is pre-release, while a deployment setting would misrepresent a product-wide lifecycle decision as an operator choice.

## Source decision

The empty hero always renders a localized Preview / 预览版 badge beneath the headline. It has no configuration switch: preview status is one product identity shared by every deployment, not a deployment-varying tunable. The badge keeps the business-tertiary background so both themes retain the product-blue context, and uses the theme's primary label token for text. That pairing gives ordinary 12px text sufficient contrast in both light and dark themes; the business-primary foreground is reserved for larger or non-text accents because it does not reach the required contrast on this background.

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/feature/2026-08-05-web-preview-product-badge.md](../02-notes/implemented/feature/2026-08-05-web-preview-product-badge.md)
- Pinned source: [.agents/notes/implemented/feature/2026-08-05-web-preview-product-badge.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/feature/2026-08-05-web-preview-product-badge.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

No implementation path could be confirmed from the pinned tree. Use the search handles below; do not invent a source location.

### Code constructs named by the note

The note names behavior or policy but no uniquely resolvable code declaration. Search by its aliases and inspect the listed package entry points.

### Tests and executable evidence

- [`apps/web/tests/markdown-inline-code-links.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/markdown-inline-code-links.e2e.ts) — A test under the owning area exercises or imports `Preview`.
- [`apps/web/tests/onboarding-deepseek-config.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/onboarding-deepseek-config.e2e.ts) — A test under the owning area exercises or imports `Preview`.
- [`packages/client/ui-trajectory/tests/table.client.spec.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-trajectory/tests/table.client.spec.tsx) — A test under the owning area exercises or imports `Preview`.
- [`apps/web/tests/snapshots/lifecycle-chrome/hero.expected.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/snapshots/lifecycle-chrome/hero.expected.md) — A test under the owning area exercises or imports `Preview`.
- [`packages/client/ui-conversation/tests/skeleton.client.spec.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/tests/skeleton.client.spec.tsx) — A test under the owning area exercises or imports `Preview`.
- [`apps/web/tests/snapshots/lifecycle-chrome/plan-active.expected.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/snapshots/lifecycle-chrome/plan-active.expected.md) — A test under the owning area exercises or imports `Preview`.
- [`apps/web/tests/snapshots/markdown-inline-code-links/ui.expected.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/snapshots/markdown-inline-code-links/ui.expected.md) — A test under the owning area exercises or imports `Preview`.
- [`packages/client/ui-settings-models/tests/components.client.spec.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-settings-models/tests/components.client.spec.tsx) — A test under the owning area exercises or imports `Preview`.
- Source verification intent: The conversation component test covers both localized badge values, and the Web lifecycle snapshots pin the English badge in the assembled empty hero.

## How to read the implementation

1. Start from the source note and run the regex below across the pinned repository.
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
- **`rust/process-discipline`** — Treat this as repository or release policy unless it changes a runtime contract. Do not add runtime machinery for a process-only lesson.

### Smallest safe implementation slice

1. Express the semantic contract as Rust types and one owning boundary.
2. Add the smallest adapter or service needed by the existing loop; do not add a new model-facing tool unless the four-tool surface cannot express the job.
3. Record durable evidence before deriving summaries or UI state.
4. Add one happy-path test, one interruption test, and one replay/restart test where state survives a turn.
5. Keep the patch reversible until the source note's strongest failure case is proven in the Rust harness.

## Search handles

- Tags: `class/feature`, `concern/evidence`, `concern/human-control`, `concern/lifecycle`, `domain/build-release`, `domain/session-state`, `domain/testing`, `lifecycle/implemented`
- Aliases: `Web preview product badge`, `feature`, `evidence`, `human control`, `lifecycle`, `build release`, `session state`, `testing`, `implemented`
- Regex: `(?i)(Web[- ]preview[- ]product[- ]badge|feature|evidence|human[- ]control|lifecycle|build[- ]release|session[- ]state|testing)`

```bash
rg -n --pcre2 "(?i)(Web[- ]preview[- ]product[- ]badge|feature|evidence|human[- ]control|lifecycle|build[- ]release|session[- ]state|testing)" source-deepseek-harness
rg -l --fixed-strings "class/feature" 03-agent-context
```

## Connected notes

- **`shares-code-with`** — [0239. Goal command input projection](0239-goal-command-input-projection.md): Shares source implementation: `packages/client/ui-conversation/tests/skeleton.client.spec.tsx`.
- **`shares-code-with`** — [0318. Fixed header, sticky composer inside the transcript scrollport](0318-fixed-header-sticky-composer-inside-the-transcript-scrollport.md): Shares source implementation: `packages/client/ui-conversation/tests/skeleton.client.spec.tsx`.
- **`shares-code-with`** — [0568. Startup slogans replace the configured TUI welcome line](0568-startup-slogans-replace-the-configured-tui-welcome-line.md): Shares source implementation: `apps/web/tests/onboarding-deepseek-config.e2e.ts`.
- **`shares-code-with`** — [0575. The banner returns, borderless](0575-the-banner-returns-borderless.md): Shares source implementation: `apps/web/tests/onboarding-deepseek-config.e2e.ts`.
- **`shares-code-with`** — [0350. Recoverable provider credential lifecycle](0350-recoverable-provider-credential-lifecycle.md): Shares source implementation: `packages/client/ui-settings-models/tests/components.client.spec.tsx`.
- **`shares-code-with`** — [0574. The banner sweeps in; the subtitle line is gone](0574-the-banner-sweeps-in-the-subtitle-line-is-gone.md): Shares source implementation: `apps/web/tests/onboarding-deepseek-config.e2e.ts`.
- **`shares-code-with`** — [0577. No startup banner](0577-no-startup-banner.md): Shares source implementation: `apps/web/tests/onboarding-deepseek-config.e2e.ts`.
- **`shares-code-with`** — [0617. Intent draft echoes in the same tick](0617-intent-draft-echoes-in-the-same-tick.md): Shares source implementation: `packages/client/ui-conversation/tests/skeleton.client.spec.tsx`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0261-web-preview-product-badge.md`.
