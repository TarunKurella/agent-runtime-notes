---
id: "dsh-note-0575"
title: "The banner returns, borderless"
status: "archived"
class: "feature"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/archived/feature/2026-07-21-tui-borderless-banner.md"
implementation_evidence: "medium"
target_anchor: "Rust provider-adapter boundary"
tags:
  - "class/feature"
  - "concern/evidence"
  - "concern/lifecycle"
  - "concern/simplification"
  - "domain/build-release"
  - "domain/configuration"
  - "domain/llm"
  - "domain/session-state"
  - "domain/shell-terminal"
  - "domain/storage"
  - "domain/testing"
  - "domain/ui-interaction"
  - "lifecycle/archived"
  - "mechanism/event-sourcing"
aliases:
  - "setStatus"
  - "HeaderComponent"
  - "side bars. Each line is a single leading space plus"
  - "DEEPSEEK HARNESS"
  - "<model>  •  <session-id>"
  - "rebuildTranscript"
  - "ui.start"
  - "detachListeners"
  - "stopBannerReveal"
  - "model • session-id"
  - "/clear"
  - "main-session-"
  - "DEEPSEEK"
  - "HARNESS"
search_regex: "(?i)(setStatus|HeaderComponent|DEEPSEEK[- ]HARNESS|<model>[- ][- ]•[- ][- ]<session\\-id>|rebuildTranscript|ui\\.start|detachListeners|stopBannerReveal)"
---

# 0575. The banner returns, borderless — implementation context

## Open this when

An intermediate no-banner design removed the boxed startup banner: it deleted HeaderComponent and its sweep, moved the model into the footer, dropped the session id, and rendered welcome as the transcript's first line. The user's verdict reversed that: bring the banner back --- "just remove the border". The four-row box frame was the objectionable chrome, not the identifying facts it carried (model, session id) nor the sweep-in motion.

## Source decision

HeaderComponent and its left-to-right sweep return, but render borderless: no ╭─╮/╰─╯ corners and no │ side bars. Each line is a single leading space plus truncateToWidth-clipped content, so the sweep's width clip can never tear an escape sequence and no fixed frame is drawn. The reveal advances through about 24 frames at 15 ms each. The header carries the title (DEEPSEEK HARNESS), a • detail line, and --- when welcome is set --- a muted subtitle. With welcome unset the header is title + detail only: there is no fixed or random slogan.

## Decision status

Historical design. Use it to understand an older pressure or failure; locate the replacing decision before implementing anything.

- Raw note: [archived/feature/2026-07-21-tui-borderless-banner.md](../02-notes/archived/feature/2026-07-21-tui-borderless-banner.md)
- Pinned source: [.agents/notes/archived/feature/2026-07-21-tui-borderless-banner.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/archived/feature/2026-07-21-tui-borderless-banner.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`.github/issue-management/policy.mjs`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.github/issue-management/policy.mjs) | repository automation | Defines `setStatus`, a construct named by the note. | `symbol-definition` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `setStatus` | `function` | [`.github/issue-management/policy.mjs:554`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.github/issue-management/policy.mjs#L554) | `async function setStatus(number, status) {` |

### Tests and executable evidence

- [`apps/web/tests/scaffold.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/scaffold.ts) — A test under the owning area exercises or imports `welcome`.
- [`apps/web/tests/remote-welcome.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/remote-welcome.e2e.ts) — A test under the owning area exercises or imports `welcome`.
- [`apps/web/tests/navigation-panes.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/navigation-panes.e2e.ts) — A test under the owning area exercises or imports `welcome`.
- [`packages/goal/tool-goal/tests/tool-goal.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/goal/tool-goal/tests/tool-goal.spec.ts) — A test under the owning area exercises or imports `setStatus`.
- [`apps/web/tests/onboarding-deepseek-config.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/onboarding-deepseek-config.e2e.ts) — A test under the owning area exercises or imports `welcome`.
- [`packages/core/agent-loop/tests/properties.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent-loop/tests/properties.spec.ts) — A test under the owning area exercises or imports `setStatus`.
- [`apps/web/tests/snapshots/plan-review/session.jsonl`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/snapshots/plan-review/session.jsonl) — A test under the owning area exercises or imports `welcome`.
- [`packages/client/web/tests/app-root.client.spec.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/web/tests/app-root.client.spec.tsx) — A test under the owning area exercises or imports `HARNESS`.
- Source verification intent: packages/ui/tui/tests/tui.spec.ts pins: the borderless banner sweeps to natural completion --- no box corners, title and main-session detail present --- with at least one clipped mid-sweep frame; a configured welcome renders the whole banner with no clipped frame; the unset-welcome banner has no subtitle; and dispose clears the sweep interval mid-sweep.

## How to read the implementation

1. Start with [`.github/issue-management/policy.mjs`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.github/issue-management/policy.mjs) because it has the strongest evidence link to the note.
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
- **`rust/process-discipline`** — Treat this as repository or release policy unless it changes a runtime contract. Do not add runtime machinery for a process-only lesson.
- **`rust/provider-adapter`** — Keep provider wire quirks inside adapters; the loop and durable event vocabulary remain provider-neutral.
- **`rust/quickjs-boundary`** — QuickJS holds orchestration variables; Rust/Tokio owns async execution and exposes only explicitly granted capabilities.
- **`rust/tokio-cancellation`** — Thread one cancellation token through async work, close owned resources before reuse, and test cancellation at every await boundary.
- **Status gate** — Historical design. Use it to understand an older pressure or failure; locate the replacing decision before implementing anything.

### Smallest safe implementation slice

1. Express the semantic contract as Rust types and one owning boundary.
2. Add the smallest adapter or service needed by the existing loop; do not add a new model-facing tool unless the four-tool surface cannot express the job.
3. Record durable evidence before deriving summaries or UI state.
4. Add one happy-path test, one interruption test, and one replay/restart test where state survives a turn.
5. Keep the patch reversible until the source note's strongest failure case is proven in the Rust harness.

## Search handles

- Tags: `class/feature`, `concern/evidence`, `concern/lifecycle`, `concern/simplification`, `domain/build-release`, `domain/configuration`, `domain/llm`, `domain/session-state`, `domain/shell-terminal`, `domain/storage`, `domain/testing`, `domain/ui-interaction`, `lifecycle/archived`, `mechanism/event-sourcing`
- Aliases: `setStatus`, `HeaderComponent`, `side bars. Each line is a single leading space plus`, `DEEPSEEK HARNESS`, `<model>  •  <session-id>`, `rebuildTranscript`, `ui.start`, `detachListeners`, `stopBannerReveal`, `model • session-id`, `/clear`, `main-session-`, `DEEPSEEK`, `HARNESS`
- Regex: `(?i)(setStatus|HeaderComponent|DEEPSEEK[- ]HARNESS|<model>[- ][- ]•[- ][- ]<session\-id>|rebuildTranscript|ui\.start|detachListeners|stopBannerReveal)`

```bash
rg -n --pcre2 "(?i)(setStatus|HeaderComponent|DEEPSEEK[- ]HARNESS|<model>[- ][- ]\u2022[- ][- ]<session\\-id>|rebuildTranscript|ui\\.start|detachListeners|stopBannerReveal)" source-deepseek-harness
rg -l --fixed-strings "class/feature" 03-agent-context
```

## Connected notes

- **`shares-code-with`** — [0568. Startup slogans replace the configured TUI welcome line](0568-startup-slogans-replace-the-configured-tui-welcome-line.md): Shares source implementation: `.github/issue-management/policy.mjs`, `apps/web/tests/navigation-panes.e2e.ts`.
- **`shares-code-with`** — [0574. The banner sweeps in; the subtitle line is gone](0574-the-banner-sweeps-in-the-subtitle-line-is-gone.md): Shares source implementation: `apps/web/tests/navigation-panes.e2e.ts`, `apps/web/tests/onboarding-deepseek-config.e2e.ts`.
- **`shares-code-with`** — [0577. No startup banner](0577-no-startup-banner.md): Shares source implementation: `apps/web/tests/navigation-panes.e2e.ts`, `apps/web/tests/onboarding-deepseek-config.e2e.ts`.
- **`shares-code-with`** — [0500. Keyless browser e2e lane for the web GUI](0500-keyless-browser-e2e-lane-for-the-web-gui.md): Shares source implementation: `apps/web/tests/scaffold.ts`.
- **`shares-code-with`** — [0684. Adopt execa for hand-rolled test subprocess plumbing](0684-adopt-execa-for-hand-rolled-test-subprocess-plumbing.md): Shares source implementation: `apps/web/tests/scaffold.ts`.
- **`shares-code-with`** — [0207. Web terminal card --- the bash render intent reaches the browser](0207-web-terminal-card-the-bash-render-intent-reaches-the-browser.md): Shares source implementation: `apps/web/tests/navigation-panes.e2e.ts`.
- **`shares-code-with`** — [0368. The overlay composer seat compensates for the bar instead of reserving a gutter](0368-the-overlay-composer-seat-compensates-for-the-bar-instead-of-reserving-a.md): Shares source implementation: `apps/web/tests/navigation-panes.e2e.ts`, `apps/web/tests/scaffold.ts`.
- **`shares-code-with`** — [0687. Agent Notes](0687-agent-notes.md): Shares source implementation: `apps/web/tests/scaffold.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0575-the-banner-returns-borderless.md`.
