---
id: "dsh-note-0629"
title: "Collapsed sidebar upper controls share one entry motion"
status: "archived"
class: "bug-fix"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/archived/bug-fix/2026-08-12-collapsed-sidebar-shared-entry-motion.md"
implementation_evidence: "medium"
target_anchor: "read/write/patch filesystem capability"
tags:
  - "class/bug-fix"
  - "concern/boundary"
  - "concern/evidence"
  - "concern/lifecycle"
  - "concern/ownership"
  - "concern/simplification"
  - "domain/configuration"
  - "domain/filesystem"
  - "domain/session-state"
  - "domain/shell-terminal"
  - "domain/ui-interaction"
  - "lifecycle/archived"
  - "mechanism/adapter"
aliases:
  - "translateX"
  - "ui-workspace"
  - "Collapsed sidebar upper controls share one entry motion"
  - "bug fix"
  - "boundary"
  - "evidence"
  - "lifecycle"
  - "ownership"
  - "simplification"
  - "configuration"
  - "filesystem"
  - "session state"
  - "shell terminal"
  - "ui interaction"
search_regex: "(?i)(translateX|ui\\-workspace|Collapsed[- ]sidebar[- ]upper[- ]controls[- ]share[- ]one[- ]entry[- ]motion|bug[- ]fix|boundary|evidence|lifecycle|ownership)"
---

# 0629. Collapsed sidebar upper controls share one entry motion — implementation context

## Open this when

The collapsed sidebar rail renders four upper controls owned by two packages: the shell owns the toggle and New Session, while the workspace region owns add and search. Their opacity timing matched, but their geometry did not. Right-aligned controls moved with the narrowing column while left-aligned controls stayed fixed, so add appeared slower than search even under the same fade. The bottom settings control has a different role. It is pinned to the rail foot and must not join the upper controls' horizontal entry.

## Source decision

At the rail settle point, the four upper 36px controls start from one left-anchored layout and share one 150ms animation from translateX(49px) to their final 10px inset. The shell applies the translation to its toggle and New Session seats and once to the workspace region, so add and search inherit the same path without nested transforms. Opacity uses the same animation timeline. The settings seat uses a separate opacity-only keyframe with the same duration and easing. A page that starts collapsed renders the rail without an entry animation, and reduced-motion mode disables both keyframes.

## Decision status

Historical design. Use it to understand an older pressure or failure; locate the replacing decision before implementing anything.

- Raw note: [archived/bug-fix/2026-08-12-collapsed-sidebar-shared-entry-motion.md](../02-notes/archived/bug-fix/2026-08-12-collapsed-sidebar-shared-entry-motion.md)
- Pinned source: [.agents/notes/archived/bug-fix/2026-08-12-collapsed-sidebar-shared-entry-motion.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/archived/bug-fix/2026-08-12-collapsed-sidebar-shared-entry-motion.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`packages/client/ui-workspace/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-workspace/src/index.ts) | package entry point | Core file in the package named by the note: `packages/client/ui-workspace`. | `named-package-member` |
| [`packages/client/ui-workspace/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-workspace/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/client/ui-workspace`. | `named-package-member` |
| [`packages/client/ui-workspace`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-workspace) | package or module directory | The note names this package or capability. | `named-package` |
| [`packages/client/ui-workspace/README.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-workspace/README.md) | package contract and examples | Core file in the package named by the note: `packages/client/ui-workspace`. | `named-package-member` |
| [`packages/client/ui-workspace/package.json`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-workspace/package.json) | composition and configuration | Core file in the package named by the note: `packages/client/ui-workspace`. | `named-package-member` |

### Code constructs named by the note

The note names behavior or policy but no uniquely resolvable code declaration. Search by its aliases and inspect the listed package entry points.

### Tests and executable evidence

- [`packages/client/ui-workspace/tests/apply.client.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-workspace/tests/apply.client.spec.ts) — A test under the owning area exercises or imports `ui-workspace`.

## How to read the implementation

1. Start with [`packages/client/ui-workspace/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-workspace/src/index.ts) because it has the strongest evidence link to the note.
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

- Tags: `class/bug-fix`, `concern/boundary`, `concern/evidence`, `concern/lifecycle`, `concern/ownership`, `concern/simplification`, `domain/configuration`, `domain/filesystem`, `domain/session-state`, `domain/shell-terminal`, `domain/ui-interaction`, `lifecycle/archived`, `mechanism/adapter`
- Aliases: `translateX`, `ui-workspace`, `Collapsed sidebar upper controls share one entry motion`, `bug fix`, `boundary`, `evidence`, `lifecycle`, `ownership`, `simplification`, `configuration`, `filesystem`, `session state`, `shell terminal`, `ui interaction`
- Regex: `(?i)(translateX|ui\-workspace|Collapsed[- ]sidebar[- ]upper[- ]controls[- ]share[- ]one[- ]entry[- ]motion|bug[- ]fix|boundary|evidence|lifecycle|ownership)`

```bash
rg -n --pcre2 "(?i)(translateX|ui\\-workspace|Collapsed[- ]sidebar[- ]upper[- ]controls[- ]share[- ]one[- ]entry[- ]motion|bug[- ]fix|boundary|evidence|lifecycle|ownership)" source-deepseek-harness
rg -l --fixed-strings "class/bug-fix" 03-agent-context
```

## Connected notes

- **`shares-code-with`** — [0472. One route to add a Workspace](0472-one-route-to-add-a-workspace.md): Shares source implementation: `packages/client/ui-workspace`, `packages/client/ui-workspace/src/index.ts`.
- **`shares-code-with`** — [0312. The scrollbar tokens get their consumer, and the workspace list reserves its gutter](0312-the-scrollbar-tokens-get-their-consumer-and-the-workspace-list-reserves.md): Shares source implementation: `packages/client/ui-workspace`, `packages/client/ui-workspace/src/index.ts`.
- **`shares-code-with`** — [0197. Web past-session search](0197-web-past-session-search.md): Shares source implementation: `packages/client/ui-workspace/README.md`.
- **`shares-code-with`** — [0275. The no-Workspace composer opens the existing picker](0275-the-no-workspace-composer-opens-the-existing-picker.md): Shares source implementation: `packages/client/ui-workspace/tests/apply.client.spec.ts`.
- **`same-design-pressure`** — [0500. Keyless browser e2e lane for the web GUI](0500-keyless-browser-e2e-lane-for-the-web-gui.md): Shares design concerns: `concern/boundary`, `concern/evidence`, `concern/lifecycle`.
- **`same-design-pressure`** — [0229. Even out the shipped tool rosters](0229-even-out-the-shipped-tool-rosters.md): Shares design concerns: `concern/boundary`, `concern/evidence`, `concern/lifecycle`.
- **`same-design-pressure`** — [0158. persistent PTY sessions](0158-persistent-pty-sessions.md): Shares design concerns: `concern/boundary`, `concern/evidence`, `concern/lifecycle`.
- **`same-design-pressure`** — [0157. Harness-level goal-based execution](0157-harness-level-goal-based-execution.md): Shares design concerns: `concern/boundary`, `concern/evidence`, `concern/lifecycle`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0629-collapsed-sidebar-upper-controls-share-one-entry-motion.md`.
