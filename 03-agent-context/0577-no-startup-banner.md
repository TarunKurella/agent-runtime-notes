---
id: "dsh-note-0577"
title: "No startup banner"
status: "archived"
class: "feature"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/archived/feature/2026-07-21-tui-no-banner.md"
implementation_evidence: "lead-only"
target_anchor: "read/write/patch filesystem capability"
tags:
  - "class/feature"
  - "concern/evidence"
  - "concern/lifecycle"
  - "concern/recovery"
  - "concern/simplification"
  - "domain/filesystem"
  - "domain/llm"
  - "domain/session-state"
  - "domain/shell-terminal"
  - "domain/storage"
  - "domain/testing"
  - "domain/ui-interaction"
  - "lifecycle/archived"
aliases:
  - "HeaderComponent"
  - "./.sessions"
  - "dsh --resume <id>"
  - "/resume"
  - "rebuildTranscript"
  - "DEEPSEEK"
  - "main-session-"
  - "/clear"
  - "packages/ui/tui/tests/tui.spec.ts"
  - "DEEPSEEK HARNESS"
  - "No startup banner"
  - "feature"
  - "evidence"
  - "lifecycle"
search_regex: "(?i)(HeaderComponent|\\./\\.sessions|dsh[- ]\\-\\-resume[- ]<id>|/resume|rebuildTranscript|DEEPSEEK|main\\-session\\-|/clear)"
---

# 0577. No startup banner — implementation context

## Open this when

The TUI opened with a boxed product banner ("DEEPSEEK HARNESS" + model/session detail), most recently with a sweep-in animation (banner sweep Agent Note). The user's verdict: remove it. A product title re-read on every boot is chrome, the box spends four rows before any content, and the identifying facts it carried (model, session) have better homes.

## Source decision

HeaderComponent, the sweep animation, and its lifecycle wiring are deleted. The TUI mounts straight into the transcript; startup renders nothing above the separator. The model name moves into the footer status line's left segment ( ↑tokens ↓tokens), so the session's driving model stays visible at all times, not just at boot. The session id is no longer displayed --- it lives in the session log and ./.sessions filenames, where dsh --resume and the /resume selector retrieve it. welcome, when configured, renders as the transcript's first line (a muted notice) inside rebuildTranscript, so palette swaps preserve it.

## Decision status

Historical design. Use it to understand an older pressure or failure; locate the replacing decision before implementing anything.

- Raw note: [archived/feature/2026-07-21-tui-no-banner.md](../02-notes/archived/feature/2026-07-21-tui-no-banner.md)
- Pinned source: [.agents/notes/archived/feature/2026-07-21-tui-no-banner.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/archived/feature/2026-07-21-tui-no-banner.md)
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
- [`packages/examples/agent-spine-demo/tests/agent-core.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/examples/agent-spine-demo/tests/agent-core.spec.ts) — A test under the owning area exercises or imports `main-session-`.
- [`packages/client/ui-settings-models/tests/apply.client.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-settings-models/tests/apply.client.spec.ts) — A test under the owning area exercises or imports `welcome`.
- Source verification intent: packages/ui/tui/tests/tui.spec.ts pins: no box corners/product title and an empty transcript when welcome is unset, with the model in the footer; a configured welcome as the first transcript line without a banner; and the welcome surviving a palette-swap transcript rebuild. The PTY smoke boots on the footer model name and asserts DEEPSEEK HARNESS is absent. Snapshots verify the full frames.

## How to read the implementation

1. Start from the source note and run the regex below across the pinned repository.
2. Read public types and entry points before implementation details. Write down the contract in plain language.
3. Follow the named constructs into their callers and tests. Confirm which component can actually enforce the rule.
4. Reconstruct one failure path: cancellation, crash, retry, stale state, partial write, or incompatible provider input—whichever the note is protecting against.
5. Only then map the contract into the current Rust repository. Target paths must come from that repository, never from guesswork in this wiki.

## Rust harness port

**Likely integration point:** read/write/patch filesystem capability.

Preserve the behavior and ownership rule. Do not mechanically translate Cordis, TypeScript declaration merging, Node streams, or package topology into Rust.

Relevant standing rules:

- **`rust/append-log`** — Keep durable facts append-only and derive views from them. Separate the synchronous fact boundary from asynchronous persistence.
- **`rust/capability-security`** — Make granted capabilities explicit. On the Windows no-admin path, probe support at runtime and fail closed rather than silently running unsandboxed.
- **`rust/four-tool-surface`** — Preserve the four model tools: read, write, patch, and exec. Reuse internal capabilities without automatically exposing new model-facing tools.
- **`rust/provider-adapter`** — Keep provider wire quirks inside adapters; the loop and durable event vocabulary remain provider-neutral.
- **`rust/tokio-cancellation`** — Thread one cancellation token through async work, close owned resources before reuse, and test cancellation at every await boundary.
- **Status gate** — Historical design. Use it to understand an older pressure or failure; locate the replacing decision before implementing anything.

### Smallest safe implementation slice

1. Express the semantic contract as Rust types and one owning boundary.
2. Add the smallest adapter or service needed by the existing loop; do not add a new model-facing tool unless the four-tool surface cannot express the job.
3. Record durable evidence before deriving summaries or UI state.
4. Add one happy-path test, one interruption test, and one replay/restart test where state survives a turn.
5. Keep the patch reversible until the source note's strongest failure case is proven in the Rust harness.

## Search handles

- Tags: `class/feature`, `concern/evidence`, `concern/lifecycle`, `concern/recovery`, `concern/simplification`, `domain/filesystem`, `domain/llm`, `domain/session-state`, `domain/shell-terminal`, `domain/storage`, `domain/testing`, `domain/ui-interaction`, `lifecycle/archived`
- Aliases: `HeaderComponent`, `./.sessions`, `dsh --resume <id>`, `/resume`, `rebuildTranscript`, `DEEPSEEK`, `main-session-`, `/clear`, `packages/ui/tui/tests/tui.spec.ts`, `DEEPSEEK HARNESS`, `No startup banner`, `feature`, `evidence`, `lifecycle`
- Regex: `(?i)(HeaderComponent|\./\.sessions|dsh[- ]\-\-resume[- ]<id>|/resume|rebuildTranscript|DEEPSEEK|main\-session\-|/clear)`

```bash
rg -n --pcre2 "(?i)(HeaderComponent|\\./\\.sessions|dsh[- ]\\-\\-resume[- ]<id>|/resume|rebuildTranscript|DEEPSEEK|main\\-session\\-|/clear)" source-deepseek-harness
rg -l --fixed-strings "class/feature" 03-agent-context
```

## Connected notes

- **`source-link`** — [0574. The banner sweeps in; the subtitle line is gone](0574-the-banner-sweeps-in-the-subtitle-line-is-gone.md): The source note links to this decision directly.
- **`source-link`** — [0575. The banner returns, borderless](0575-the-banner-returns-borderless.md): The source note links to this decision directly.
- **`shares-code-with`** — [0568. Startup slogans replace the configured TUI welcome line](0568-startup-slogans-replace-the-configured-tui-welcome-line.md): Shares source implementation: `apps/web/tests/navigation-panes.e2e.ts`, `apps/web/tests/onboarding-deepseek-config.e2e.ts`.
- **`shares-code-with`** — [0500. Keyless browser e2e lane for the web GUI](0500-keyless-browser-e2e-lane-for-the-web-gui.md): Shares source implementation: `apps/web/tests/scaffold.ts`.
- **`shares-code-with`** — [0207. Web terminal card --- the bash render intent reaches the browser](0207-web-terminal-card-the-bash-render-intent-reaches-the-browser.md): Shares source implementation: `apps/web/tests/navigation-panes.e2e.ts`.
- **`shares-code-with`** — [0684. Adopt execa for hand-rolled test subprocess plumbing](0684-adopt-execa-for-hand-rolled-test-subprocess-plumbing.md): Shares source implementation: `apps/web/tests/scaffold.ts`.
- **`shares-code-with`** — [0611. Keep the Code Mode result card complete](0611-keep-the-code-mode-result-card-complete.md): Shares source implementation: `apps/web/tests/scaffold.ts`.
- **`shares-code-with`** — [0368. The overlay composer seat compensates for the bar instead of reserving a gutter](0368-the-overlay-composer-seat-compensates-for-the-bar-instead-of-reserving-a.md): Shares source implementation: `apps/web/tests/navigation-panes.e2e.ts`, `apps/web/tests/scaffold.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0577-no-startup-banner.md`.
