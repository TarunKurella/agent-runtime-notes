---
id: "dsh-note-0254"
title: "Web slash-command fuzzy discovery"
status: "implemented"
class: "feature"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/feature/2026-08-04-web-slash-command-fuzzy-discovery.md"
implementation_evidence: "lead-only"
target_anchor: "append-only event and session store"
tags:
  - "class/feature"
  - "concern/discovery-routing"
  - "concern/evidence"
  - "concern/performance"
  - "concern/schema-types"
  - "domain/build-release"
  - "domain/configuration"
  - "domain/session-state"
  - "domain/storage"
  - "lifecycle/implemented"
  - "mechanism/event-sourcing"
  - "mechanism/projection"
aliases:
  - "/cpt"
  - "/compact"
  - "Web slash-command fuzzy discovery"
  - "feature"
  - "discovery routing"
  - "evidence"
  - "performance"
  - "schema types"
  - "build release"
  - "configuration"
  - "session state"
  - "storage"
  - "implemented"
  - "event sourcing"
search_regex: "(?i)(/cpt|/compact|Web[- ]slash\\-command[- ]fuzzy[- ]discovery|feature|discovery[- ]routing|evidence|performance|schema[- ]types)"
---

# 0254. Web slash-command fuzzy discovery — implementation context

## Open this when

The web command menu required a command-name prefix, so discovery failed when a user remembered the significant letters but not their exact positions. Broadening menu matching could make discovery easier, but command execution must remain exact and deterministic: an approximate line must never execute a nearby command.

## Source decision

The / command source fuzzy-matches the typed query against command names as a case-insensitive ordered subsequence. Exact prefixes form the highest ranking class. Within each class, the strongest alignment score rewards separator boundaries and adjacent characters while penalizing leading characters and gaps; equal scores retain the host-directory and client-contribution order. Position filtering still removes argument-taking commands from inline menus before ranking. The scorer uses dynamic programming in O(query length × name length) time and O(name length) memory per candidate.

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/feature/2026-08-04-web-slash-command-fuzzy-discovery.md](../02-notes/implemented/feature/2026-08-04-web-slash-command-fuzzy-discovery.md)
- Pinned source: [.agents/notes/implemented/feature/2026-08-04-web-slash-command-fuzzy-discovery.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/feature/2026-08-04-web-slash-command-fuzzy-discovery.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`scripts/dev-web.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/dev-web.ts) | repository automation | Path shares title concepts: web. | `title-path-lead` |
| [`docs/web-styling.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/web-styling.md) | package contract and examples | Path shares title concepts: web. | `title-path-lead` |
| [`apps/web/index.html`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/index.html) | supporting file | Path shares title concepts: web. | `title-path-lead` |
| [`vitest.web.config.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vitest.web.config.ts) | runtime implementation | Path shares title concepts: web. | `title-path-lead` |
| [`apps/web/src/main.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/src/main.ts) | runtime implementation | Path shares title concepts: web. | `title-path-lead` |
| [`apps/web/package.json`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/package.json) | composition and configuration | Path shares title concepts: web. | `title-path-lead` |
| [`docs/web-styling.zh.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/web-styling.zh.md) | package contract and examples | Path shares title concepts: web. | `title-path-lead` |
| [`apps/web/tsconfig.json`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tsconfig.json) | composition and configuration | Path shares title concepts: web. | `title-path-lead` |
| [`docs/subsystems/web.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/subsystems/web.md) | package contract and examples | Path shares title concepts: web. | `title-path-lead` |
| [`packages/web/README.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/web/README.md) | package contract and examples | Path shares title concepts: web. | `title-path-lead` |
| [`packages/web/AGENTS.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/web/AGENTS.md) | package contract and examples | Path shares title concepts: web. | `title-path-lead` |
| [`apps/web/vite.config.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/vite.config.ts) | runtime implementation | Path shares title concepts: web. | `title-path-lead` |

### Code constructs named by the note

The note names behavior or policy but no uniquely resolvable code declaration. Search by its aliases and inspect the listed package entry points.

### Tests and executable evidence

- [`apps/web/tests/snapshots/lifecycle-chrome/command-menu-fuzzy.expected.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/snapshots/lifecycle-chrome/command-menu-fuzzy.expected.md) — Path shares title concepts: fuzzy, web.
- [`scripts/dev-web.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/dev-web.spec.ts) — Path shares title concepts: web.
- [`apps/web/tests/README.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/README.md) — Path shares title concepts: web.
- [`apps/web/tests/support.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/support.ts) — Path shares title concepts: web.
- [`apps/web/tests/scaffold.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/scaffold.ts) — Path shares title concepts: web.
- [`apps/web/tests/README.zh.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/README.zh.md) — Path shares title concepts: web.
- [`apps/web/tests/goal-bar.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/goal-bar.e2e.ts) — Path shares title concepts: web.
- [`apps/web/tests/steering.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/steering.e2e.ts) — Path shares title concepts: web.

## How to read the implementation

1. Start with [`scripts/dev-web.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/dev-web.ts) because it has the strongest evidence link to the note.
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

### Smallest safe implementation slice

1. Express the semantic contract as Rust types and one owning boundary.
2. Add the smallest adapter or service needed by the existing loop; do not add a new model-facing tool unless the four-tool surface cannot express the job.
3. Record durable evidence before deriving summaries or UI state.
4. Add one happy-path test, one interruption test, and one replay/restart test where state survives a turn.
5. Keep the patch reversible until the source note's strongest failure case is proven in the Rust harness.

## Search handles

- Tags: `class/feature`, `concern/discovery-routing`, `concern/evidence`, `concern/performance`, `concern/schema-types`, `domain/build-release`, `domain/configuration`, `domain/session-state`, `domain/storage`, `lifecycle/implemented`, `mechanism/event-sourcing`, `mechanism/projection`
- Aliases: `/cpt`, `/compact`, `Web slash-command fuzzy discovery`, `feature`, `discovery routing`, `evidence`, `performance`, `schema types`, `build release`, `configuration`, `session state`, `storage`, `implemented`, `event sourcing`
- Regex: `(?i)(/cpt|/compact|Web[- ]slash\-command[- ]fuzzy[- ]discovery|feature|discovery[- ]routing|evidence|performance|schema[- ]types)`

```bash
rg -n --pcre2 "(?i)(/cpt|/compact|Web[- ]slash\\-command[- ]fuzzy[- ]discovery|feature|discovery[- ]routing|evidence|performance|schema[- ]types)" source-deepseek-harness
rg -l --fixed-strings "class/feature" 03-agent-context
```

## Connected notes

- **`same-design-pressure`** — [0045. Web client architecture --- the client cordis plugin tree, the slot system, and the React-free object layer](0045-web-client-architecture-the-client-cordis-plugin-tree-the-slot-system-an.md): Shares design concerns: `concern/discovery-routing`, `concern/evidence`, `concern/performance`.
- **`same-design-pressure`** — [0500. Keyless browser e2e lane for the web GUI](0500-keyless-browser-e2e-lane-for-the-web-gui.md): Shares design concerns: `concern/discovery-routing`, `concern/evidence`, `concern/performance`.
- **`same-design-pressure`** — [0198. Web subagent catalog and human continuation](0198-web-subagent-catalog-and-human-continuation.md): Shares design concerns: `concern/discovery-routing`, `concern/evidence`, `concern/schema-types`.
- **`same-design-pressure`** — [0182. Session model selection in the Web composer](0182-session-model-selection-in-the-web-composer.md): Shares design concerns: `concern/discovery-routing`, `concern/evidence`, `concern/performance`.
- **`same-design-pressure`** — [0063. Web input state machine, composer slots, and the slash pipeline (ui-conversation input / ui-input-trigger)](0063-web-input-state-machine-composer-slots-and-the-slash-pipeline-ui-convers.md): Shares design concerns: `concern/discovery-routing`, `concern/evidence`, `concern/performance`.
- **`same-design-pressure`** — [0564. Bash-backed grep and glob discovery tools](0564-bash-backed-grep-and-glob-discovery-tools.md): Shares design concerns: `concern/discovery-routing`, `concern/evidence`, `concern/performance`.
- **`same-design-pressure`** — [0158. persistent PTY sessions](0158-persistent-pty-sessions.md): Shares design concerns: `concern/discovery-routing`, `concern/evidence`, `concern/performance`.
- **`same-design-pressure`** — [0142. Dynamic workflows --- a script-driven multi-agent orchestration seam](0142-dynamic-workflows-a-script-driven-multi-agent-orchestration-seam.md): Shares design concerns: `concern/discovery-routing`, `concern/evidence`, `concern/performance`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0254-web-slash-command-fuzzy-discovery.md`.
