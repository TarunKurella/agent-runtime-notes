---
id: "dsh-note-0624"
title: "Web details default closed"
status: "archived"
class: "bug-fix"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/archived/bug-fix/2026-07-30-web-details-default-closed.md"
implementation_evidence: "medium"
target_anchor: "append-only event and session store"
tags:
  - "class/bug-fix"
  - "concern/boundary"
  - "concern/discovery-routing"
  - "concern/evidence"
  - "concern/lifecycle"
  - "concern/ownership"
  - "concern/simplification"
  - "domain/session-state"
  - "domain/storage"
  - "domain/ui-interaction"
  - "domain/web-retrieval"
  - "lifecycle/archived"
  - "mechanism/projection"
aliases:
  - "AppFrame"
  - "openDetails"
  - "Web details default closed"
  - "bug fix"
  - "boundary"
  - "discovery routing"
  - "evidence"
  - "lifecycle"
  - "ownership"
  - "simplification"
  - "session state"
  - "storage"
  - "ui interaction"
  - "web retrieval"
search_regex: "(?i)(AppFrame|openDetails|Web[- ]details[- ]default[- ]closed|bug[- ]fix|boundary|discovery[- ]routing|evidence|lifecycle)"
---

# 0624. Web details default closed — implementation context

## Open this when

The transient layout store initialized details to its 360px contract width. The first connected Session and every full reload therefore reserved a right column before the user selected any detail content. Chat tool rows deliberately remain inline and do not open details, while Trajectory rows open the panel when an event is selected, so an open layout default did not represent an active detail selection.

## Source decision

The layout store initializes details to zero while retaining the existing 360px contract default for openDetails(). AppFrame keeps the details slot mounted at zero width, so an explicit entry point such as Trajectory event selection can open the panel without remounting its subtree. The Session ownership lifecycle remains authoritative: unselected surfaces derive zero width without taking ownership, returning to the same Session preserves an explicitly opened width, and selecting a different Session closes it. Panel geometry remains transient.

## Decision status

Historical design. Use it to understand an older pressure or failure; locate the replacing decision before implementing anything.

- Raw note: [archived/bug-fix/2026-07-30-web-details-default-closed.md](../02-notes/archived/bug-fix/2026-07-30-web-details-default-closed.md)
- Pinned source: [.agents/notes/archived/bug-fix/2026-07-30-web-details-default-closed.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/archived/bug-fix/2026-07-30-web-details-default-closed.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`packages/client/ui-layout/src/client/AppFrame.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-layout/src/client/AppFrame.tsx) | runtime implementation | Defines `AppFrame`, a construct named by the note. | `symbol-definition` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `AppFrame` | `function` | [`packages/client/ui-layout/src/client/AppFrame.tsx:87`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-layout/src/client/AppFrame.tsx#L87) | `export function AppFrame({` |

### Tests and executable evidence

- [`apps/web/tests/smoke-real.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/smoke-real.e2e.ts) — A test under the owning area exercises or imports `openDetails`.
- [`packages/client/web/tests/app-shell.client.spec.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/web/tests/app-shell.client.spec.tsx) — A test under the owning area exercises or imports `openDetails`.
- [`packages/client/ui-layout/tests/apply.client.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-layout/tests/apply.client.spec.ts) — A test under the owning area exercises or imports `openDetails`. A test under the owning area exercises or imports `AppFrame`.
- [`packages/client/ui-layout/tests/service.client.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-layout/tests/service.client.spec.ts) — A test under the owning area exercises or imports `openDetails`.
- [`packages/client/ui-layout/tests/app-frame.client.spec.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-layout/tests/app-frame.client.spec.tsx) — A test under the owning area exercises or imports `openDetails`. A test under the owning area exercises or imports `AppFrame`.
- [`packages/client/ui-layout/tests/layout-store.client.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-layout/tests/layout-store.client.spec.ts) — A test under the owning area exercises or imports `openDetails`.
- [`packages/client/ui-tool/tests/toolview-slot.client.spec.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-tool/tests/toolview-slot.client.spec.tsx) — A test under the owning area exercises or imports `openDetails`.
- [`packages/client/ui-tool/tests/assembly-surfaces.client.spec.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-tool/tests/assembly-surfaces.client.spec.tsx) — A test under the owning area exercises or imports `openDetails`.

## How to read the implementation

1. Start with [`packages/client/ui-layout/src/client/AppFrame.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-layout/src/client/AppFrame.tsx) because it has the strongest evidence link to the note.
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
- **Status gate** — Historical design. Use it to understand an older pressure or failure; locate the replacing decision before implementing anything.

### Smallest safe implementation slice

1. Express the semantic contract as Rust types and one owning boundary.
2. Add the smallest adapter or service needed by the existing loop; do not add a new model-facing tool unless the four-tool surface cannot express the job.
3. Record durable evidence before deriving summaries or UI state.
4. Add one happy-path test, one interruption test, and one replay/restart test where state survives a turn.
5. Keep the patch reversible until the source note's strongest failure case is proven in the Rust harness.

## Search handles

- Tags: `class/bug-fix`, `concern/boundary`, `concern/discovery-routing`, `concern/evidence`, `concern/lifecycle`, `concern/ownership`, `concern/simplification`, `domain/session-state`, `domain/storage`, `domain/ui-interaction`, `domain/web-retrieval`, `lifecycle/archived`, `mechanism/projection`
- Aliases: `AppFrame`, `openDetails`, `Web details default closed`, `bug fix`, `boundary`, `discovery routing`, `evidence`, `lifecycle`, `ownership`, `simplification`, `session state`, `storage`, `ui interaction`, `web retrieval`
- Regex: `(?i)(AppFrame|openDetails|Web[- ]details[- ]default[- ]closed|bug[- ]fix|boundary|discovery[- ]routing|evidence|lifecycle)`

```bash
rg -n --pcre2 "(?i)(AppFrame|openDetails|Web[- ]details[- ]default[- ]closed|bug[- ]fix|boundary|discovery[- ]routing|evidence|lifecycle)" source-deepseek-harness
rg -l --fixed-strings "class/bug-fix" 03-agent-context
```

## Connected notes

- **`shares-code-with`** — [0319. Web details follow the current Session lifecycle](0319-web-details-follow-the-current-session-lifecycle.md): Shares source implementation: `packages/client/ui-layout/src/client/AppFrame.tsx`, `packages/client/ui-layout/tests/apply.client.spec.ts`.
- **`shares-code-with`** — [0598. Address pending queue occurrences for edit and removal](0598-address-pending-queue-occurrences-for-edit-and-removal.md): Shares source implementation: `apps/web/tests/smoke-real.e2e.ts`.
- **`shares-code-with`** — [0239. Goal command input projection](0239-goal-command-input-projection.md): Shares source implementation: `packages/client/ui-conversation/tests/chat-view.client.spec.tsx`.
- **`shares-code-with`** — [0684. Adopt execa for hand-rolled test subprocess plumbing](0684-adopt-execa-for-hand-rolled-test-subprocess-plumbing.md): Shares source implementation: `apps/web/tests/smoke-real.e2e.ts`.
- **`shares-code-with`** — [0611. Keep the Code Mode result card complete](0611-keep-the-code-mode-result-card-complete.md): Shares source implementation: `apps/web/tests/smoke-real.e2e.ts`.
- **`shares-code-with`** — [0359. Pre-Plugin Theme Bootstrap](0359-pre-plugin-theme-bootstrap.md): Shares source implementation: `apps/web/tests/smoke-real.e2e.ts`.
- **`shares-code-with`** — [0318. Fixed header, sticky composer inside the transcript scrollport](0318-fixed-header-sticky-composer-inside-the-transcript-scrollport.md): Shares source implementation: `packages/client/ui-conversation/tests/chat-view.client.spec.tsx`.
- **`shares-code-with`** — [0296. Status-driven disclosure for workflow runs](0296-status-driven-disclosure-for-workflow-runs.md): Shares source implementation: `apps/web/tests/smoke-real.e2e.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0624-web-details-default-closed.md`.
