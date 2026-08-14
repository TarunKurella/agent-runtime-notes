---
id: "dsh-note-0574"
title: "The banner sweeps in; the subtitle line is gone"
status: "archived"
class: "feature"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/archived/feature/2026-07-21-tui-banner-sweep.md"
implementation_evidence: "lead-only"
target_anchor: "Rust provider-adapter boundary"
tags:
  - "class/feature"
  - "concern/boundary"
  - "concern/evidence"
  - "concern/lifecycle"
  - "concern/ownership"
  - "domain/configuration"
  - "domain/llm"
  - "domain/session-state"
  - "domain/shell-terminal"
  - "domain/storage"
  - "domain/testing"
  - "domain/ui-interaction"
  - "lifecycle/archived"
aliases:
  - "pickStartupSlogan"
  - "HeaderComponent"
  - "revealWidth"
  - "ui.start"
  - "detachListeners"
  - "stopBannerReveal"
  - ") to the banner's top-right corner ("
  - "truncateToWidth"
  - "STARTUP_SLOGANS"
  - "packages/ui/tui/tests/tui.spec.ts"
  - "The banner sweeps in; the subtitle line is gone"
  - "feature"
  - "boundary"
  - "evidence"
search_regex: "(?i)(pickStartupSlogan|HeaderComponent|revealWidth|ui\\.start|detachListeners|stopBannerReveal|\\)[- ]to[- ]the[- ]banner's[- ]top\\-right[- ]corner[- ]\\(|truncateToWidth)"
---

# 0574. The banner sweeps in; the subtitle line is gone — implementation context

## Open this when

The startup-slogans Agent Note replaced the instructional welcome line with a random slogan bank revealed by a per-character typewriter. In use the quotes read as weird --- random flavor text in a tool's header --- and the animation was slow (40 ms/char over a full sentence) while animating only one line of a four-line banner. This note supersedes that decision's slogan half; the removal of the configured demo welcome and the animation-lifecycle groundwork stand.

## Source decision

The slogan bank, pickStartupSlogan, and the typewriter reveal are deleted. When welcome is unset the banner simply has no subtitle line --- title and model/session detail only. The welcome config remains for deployments and fixtures that want a fixed subtitle, rendered frame-deterministically with no animation. The startup animation is now the whole banner: HeaderComponent gains a revealWidth clip, and the header box wipes in left-to-right over ~24 frames at 15 ms (~360 ms total, ~60 fps), started after ui.start() succeeds and cleared through the same detachListeners path the typewriter used.

## Decision status

Historical design. Use it to understand an older pressure or failure; locate the replacing decision before implementing anything.

- Raw note: [archived/feature/2026-07-21-tui-banner-sweep.md](../02-notes/archived/feature/2026-07-21-tui-banner-sweep.md)
- Pinned source: [.agents/notes/archived/feature/2026-07-21-tui-banner-sweep.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/archived/feature/2026-07-21-tui-banner-sweep.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

No implementation path could be confirmed from the pinned tree. Use the search handles below; do not invent a source location.

### Code constructs named by the note

The note names behavior or policy but no uniquely resolvable code declaration. Search by its aliases and inspect the listed package entry points.

### Tests and executable evidence

- [`apps/web/tests/scaffold.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/scaffold.ts) — A test under the owning area exercises or imports `welcome`.
- [`apps/web/tests/remote-welcome.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/remote-welcome.e2e.ts) — A test under the owning area exercises or imports `welcome`.
- [`apps/web/tests/navigation-panes.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/navigation-panes.e2e.ts) — A test under the owning area exercises or imports `welcome`.
- [`apps/web/tests/onboarding-deepseek-config.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/onboarding-deepseek-config.e2e.ts) — A test under the owning area exercises or imports `welcome`.
- [`apps/web/tests/snapshots/plan-review/session.jsonl`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/snapshots/plan-review/session.jsonl) — A test under the owning area exercises or imports `welcome`.
- [`apps/web/tests/snapshots/plan-review/review.expected.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/snapshots/plan-review/review.expected.md) — A test under the owning area exercises or imports `welcome`.
- [`packages/client/ui-settings-models/tests/apply.client.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-settings-models/tests/apply.client.spec.ts) — A test under the owning area exercises or imports `welcome`.
- [`packages/client/ui-settings-general/tests/shell.client.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-settings-general/tests/shell.client.spec.ts) — A test under the owning area exercises or imports `welcome`.
- Source verification intent: packages/ui/tui/tests/tui.spec.ts pins: the sweep completes to a full banner (both corners + title) and produced at least one clipped mid-sweep frame; a configured welcome renders verbatim with no clipped frames; the unset-welcome banner has no subtitle; and dispose clears the sweep's own interval handle. The PTY smoke boots on the ╮ completion marker across the tui-demo bin, the dsh CLI, and the personal-overlay scenarios. Verified live in tmux.

## How to read the implementation

1. Start from the source note and run the regex below across the pinned repository.
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
- **`rust/tokio-cancellation`** — Thread one cancellation token through async work, close owned resources before reuse, and test cancellation at every await boundary.
- **`rust/trait-seam`** — Use a small trait at the capability boundary; keep policy and model-facing schemas outside the backend implementation.
- **Status gate** — Historical design. Use it to understand an older pressure or failure; locate the replacing decision before implementing anything.

### Smallest safe implementation slice

1. Express the semantic contract as Rust types and one owning boundary.
2. Add the smallest adapter or service needed by the existing loop; do not add a new model-facing tool unless the four-tool surface cannot express the job.
3. Record durable evidence before deriving summaries or UI state.
4. Add one happy-path test, one interruption test, and one replay/restart test where state survives a turn.
5. Keep the patch reversible until the source note's strongest failure case is proven in the Rust harness.

## Search handles

- Tags: `class/feature`, `concern/boundary`, `concern/evidence`, `concern/lifecycle`, `concern/ownership`, `domain/configuration`, `domain/llm`, `domain/session-state`, `domain/shell-terminal`, `domain/storage`, `domain/testing`, `domain/ui-interaction`, `lifecycle/archived`
- Aliases: `pickStartupSlogan`, `HeaderComponent`, `revealWidth`, `ui.start`, `detachListeners`, `stopBannerReveal`, `) to the banner's top-right corner (`, `truncateToWidth`, `STARTUP_SLOGANS`, `packages/ui/tui/tests/tui.spec.ts`, `The banner sweeps in; the subtitle line is gone`, `feature`, `boundary`, `evidence`
- Regex: `(?i)(pickStartupSlogan|HeaderComponent|revealWidth|ui\.start|detachListeners|stopBannerReveal|\)[- ]to[- ]the[- ]banner's[- ]top\-right[- ]corner[- ]\(|truncateToWidth)`

```bash
rg -n --pcre2 "(?i)(pickStartupSlogan|HeaderComponent|revealWidth|ui\\.start|detachListeners|stopBannerReveal|\\)[- ]to[- ]the[- ]banner's[- ]top\\-right[- ]corner[- ]\\(|truncateToWidth)" source-deepseek-harness
rg -l --fixed-strings "class/feature" 03-agent-context
```

## Connected notes

- **`source-link`** — [0568. Startup slogans replace the configured TUI welcome line](0568-startup-slogans-replace-the-configured-tui-welcome-line.md): The source note links to this decision directly.
- **`source-link`** — [0577. No startup banner](0577-no-startup-banner.md): The source note links to this decision directly.
- **`shares-code-with`** — [0575. The banner returns, borderless](0575-the-banner-returns-borderless.md): Shares source implementation: `apps/web/tests/navigation-panes.e2e.ts`, `apps/web/tests/onboarding-deepseek-config.e2e.ts`.
- **`shares-code-with`** — [0349. onboarding takeover chrome moves into the step](0349-onboarding-takeover-chrome-moves-into-the-step.md): Shares source implementation: `apps/web/tests/onboarding-deepseek-config.e2e.ts`, `packages/client/ui-settings-general/tests/settings-root.client.spec.tsx`.
- **`shares-code-with`** — [0500. Keyless browser e2e lane for the web GUI](0500-keyless-browser-e2e-lane-for-the-web-gui.md): Shares source implementation: `apps/web/tests/scaffold.ts`.
- **`shares-code-with`** — [0684. Adopt execa for hand-rolled test subprocess plumbing](0684-adopt-execa-for-hand-rolled-test-subprocess-plumbing.md): Shares source implementation: `apps/web/tests/scaffold.ts`.
- **`shares-code-with`** — [0207. Web terminal card --- the bash render intent reaches the browser](0207-web-terminal-card-the-bash-render-intent-reaches-the-browser.md): Shares source implementation: `apps/web/tests/navigation-panes.e2e.ts`.
- **`shares-code-with`** — [0368. The overlay composer seat compensates for the bar instead of reserving a gutter](0368-the-overlay-composer-seat-compensates-for-the-bar-instead-of-reserving-a.md): Shares source implementation: `apps/web/tests/navigation-panes.e2e.ts`, `apps/web/tests/scaffold.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0574-the-banner-sweeps-in-the-subtitle-line-is-gone.md`.
