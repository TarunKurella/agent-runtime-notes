---
id: "dsh-note-0568"
title: "Startup slogans replace the configured TUI welcome line"
status: "archived"
class: "feature"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/archived/feature/2026-07-20-tui-startup-slogans.md"
implementation_evidence: "medium"
target_anchor: "append-only event and session store"
tags:
  - "class/feature"
  - "concern/boundary"
  - "concern/discovery-routing"
  - "concern/evidence"
  - "concern/lifecycle"
  - "concern/ownership"
  - "concern/schema-types"
  - "domain/build-release"
  - "domain/configuration"
  - "domain/extensions"
  - "domain/session-state"
  - "domain/shell-terminal"
  - "domain/storage"
  - "domain/testing"
  - "domain/ui-interaction"
  - "lifecycle/archived"
  - "mechanism/projection"
aliases:
  - "setStatus"
  - "ui.start"
  - "detachListeners"
  - "ready."
  - "examples/tui-agent/cordis.yml"
  - "dsh-tui"
  - "STARTUP_SLOGANS"
  - "pickStartupSlogan"
  - "block cursor trailing until complete. The reveal starts only after"
  - "dsh-tui-demo"
  - "packages/ui/tui/src/index.ts"
  - "applyColorScheme"
  - ".then"
  - ".catch"
search_regex: "(?i)(setStatus|ui\\.start|detachListeners|ready\\.|examples/tui\\-agent/cordis\\.yml|dsh\\-tui|STARTUP_SLOGANS|pickStartupSlogan)"
---

# 0568. Startup slogans replace the configured TUI welcome line — implementation context

## Open this when

The TUI header subtitle came from a welcome config the demo leaf set to "TUI agent ready. Give it a coding task." --- instructional filler that told a returning user nothing, restated what the product is on every boot, and had a hardcoded twin ('ready.') as the schema default in two packages. The product wanted a startup moment with some character instead of a static banner caption.

## Source decision

examples/tui-agent/cordis.yml no longer configures welcome; the config key stays for deployments and fixtures that need a fixed, deterministic subtitle (the Code Mode overlay and every snapshot/scripted fixture keep theirs). When welcome is unset, dsh-tui picks one member of an exported STARTUP_SLOGANS bank per boot (pickStartupSlogan, injectable random source) and reveals it with a typewriter animation: one character per 40 ms frame, a ▌ block cursor trailing until complete. The reveal starts only after ui.start() succeeds and its interval is cleared on dispose alongside the other listeners.

## Decision status

Historical design. Use it to understand an older pressure or failure; locate the replacing decision before implementing anything.

- Raw note: [archived/feature/2026-07-20-tui-startup-slogans.md](../02-notes/archived/feature/2026-07-20-tui-startup-slogans.md)
- Pinned source: [.agents/notes/archived/feature/2026-07-20-tui-startup-slogans.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/archived/feature/2026-07-20-tui-startup-slogans.md)
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
- [`scripts/gen-third-party-notices.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/gen-third-party-notices.spec.ts) — A test under the owning area exercises or imports `dsh-tui`. Contains the exact code literal `dsh-tui` named by the note.
- [`packages/goal/tool-goal/tests/tool-goal.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/goal/tool-goal/tests/tool-goal.spec.ts) — A test under the owning area exercises or imports `setStatus`.
- [`apps/web/tests/onboarding-deepseek-config.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/onboarding-deepseek-config.e2e.ts) — A test under the owning area exercises or imports `welcome`.
- [`packages/core/agent-loop/tests/properties.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent-loop/tests/properties.spec.ts) — A test under the owning area exercises or imports `setStatus`.
- [`apps/web/tests/snapshots/plan-review/session.jsonl`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/snapshots/plan-review/session.jsonl) — A test under the owning area exercises or imports `welcome`.
- Source verification intent: packages/ui/tui/tests/tui.spec.ts pins deterministic bank selection with an injected random source, the reveal (a bank member fully rendered, cursor frames observed), the configured-welcome path rendering verbatim with no cursor, and dispose stopping a mid-reveal animation. examples/tui-agent/tests/tui-keyless-smoke.e2e.ts boots the real tree in a PTY and waits on the reveal cursor. Verified live in tmux (mid-reveal frame no map below▌ then the full slogan).

## How to read the implementation

1. Start with [`.github/issue-management/policy.mjs`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.github/issue-management/policy.mjs) because it has the strongest evidence link to the note.
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
- **`rust/newtype-ids`** — Represent identity domains with Rust newtypes; do not pass bare strings across session, call, turn, step, and agent boundaries.
- **`rust/process-discipline`** — Treat this as repository or release policy unless it changes a runtime contract. Do not add runtime machinery for a process-only lesson.
- **`rust/quickjs-boundary`** — QuickJS holds orchestration variables; Rust/Tokio owns async execution and exposes only explicitly granted capabilities.
- **`rust/serde-validation`** — Parse untrusted input into strict serde types and validate semantic constraints before executing side effects.
- **`rust/tagged-enums`** — Use serde-tagged enums for a closed wire vocabulary. Add an explicit extension envelope only where runtime extension is truly required.
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

- Tags: `class/feature`, `concern/boundary`, `concern/discovery-routing`, `concern/evidence`, `concern/lifecycle`, `concern/ownership`, `concern/schema-types`, `domain/build-release`, `domain/configuration`, `domain/extensions`, `domain/session-state`, `domain/shell-terminal`, `domain/storage`, `domain/testing`, `domain/ui-interaction`, `lifecycle/archived`, `mechanism/projection`
- Aliases: `setStatus`, `ui.start`, `detachListeners`, `ready.`, `examples/tui-agent/cordis.yml`, `dsh-tui`, `STARTUP_SLOGANS`, `pickStartupSlogan`, `block cursor trailing until complete. The reveal starts only after`, `dsh-tui-demo`, `packages/ui/tui/src/index.ts`, `applyColorScheme`, `.then`, `.catch`
- Regex: `(?i)(setStatus|ui\.start|detachListeners|ready\.|examples/tui\-agent/cordis\.yml|dsh\-tui|STARTUP_SLOGANS|pickStartupSlogan)`

```bash
rg -n --pcre2 "(?i)(setStatus|ui\\.start|detachListeners|ready\\.|examples/tui\\-agent/cordis\\.yml|dsh\\-tui|STARTUP_SLOGANS|pickStartupSlogan)" source-deepseek-harness
rg -l --fixed-strings "class/feature" 03-agent-context
```

## Connected notes

- **`source-link`** — [0574. The banner sweeps in; the subtitle line is gone](0574-the-banner-sweeps-in-the-subtitle-line-is-gone.md): The source note links to this decision directly.
- **`shares-code-with`** — [0575. The banner returns, borderless](0575-the-banner-returns-borderless.md): Shares source implementation: `.github/issue-management/policy.mjs`, `apps/web/tests/navigation-panes.e2e.ts`.
- **`shares-code-with`** — [0577. No startup banner](0577-no-startup-banner.md): Shares source implementation: `apps/web/tests/navigation-panes.e2e.ts`, `apps/web/tests/onboarding-deepseek-config.e2e.ts`.
- **`shares-code-with`** — [0500. Keyless browser e2e lane for the web GUI](0500-keyless-browser-e2e-lane-for-the-web-gui.md): Shares source implementation: `apps/web/tests/scaffold.ts`.
- **`shares-code-with`** — [0684. Adopt execa for hand-rolled test subprocess plumbing](0684-adopt-execa-for-hand-rolled-test-subprocess-plumbing.md): Shares source implementation: `apps/web/tests/scaffold.ts`.
- **`shares-code-with`** — [0359. Pre-Plugin Theme Bootstrap](0359-pre-plugin-theme-bootstrap.md): Shares source implementation: `apps/web/tests/scaffold.ts`.
- **`shares-code-with`** — [0393. Project canonical documentation into the website](0393-project-canonical-documentation-into-the-website.md): Shares source implementation: `scripts/gen-third-party-notices.spec.ts`.
- **`shares-code-with`** — [0314. Web GUI changes close the loop on the existing URL](0314-web-gui-changes-close-the-loop-on-the-existing-url.md): Shares source implementation: `apps/web/tests/scaffold.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0568-startup-slogans-replace-the-configured-tui-welcome-line.md`.
