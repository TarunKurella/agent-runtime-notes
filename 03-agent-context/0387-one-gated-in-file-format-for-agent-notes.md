---
id: "dsh-note-0387"
title: "One gated in-file format for Agent Notes"
status: "implemented"
class: "process"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/process/2026-07-05-uniform-agent-note-format.md"
implementation_evidence: "high"
target_anchor: "repository tests and engineering policy"
tags:
  - "class/process"
  - "concern/boundary"
  - "concern/evidence"
  - "concern/lifecycle"
  - "concern/ownership"
  - "concern/performance"
  - "domain/build-release"
  - "domain/session-state"
  - "domain/testing"
  - "lifecycle/implemented"
  - "mechanism/policy"
aliases:
  - "Context"
  - "# Agent Note: <title>"
  - "proposed/"
  - "implemented/"
  - "rejected/"
  - "pnpm run verify-agent-note-format"
  - "doc-sync"
  - "agent-note-format: alternatives-not-recorded"
  - "Agent Note:"
  - "## What we give up"
  - "FORMAT.md"
  - "One gated in-file format for Agent Notes"
  - "process"
  - "boundary"
search_regex: "(?i)(Context|\\#[- ]Agent[- ]Note:[- ]<title>|proposed/|implemented/|rejected/|pnpm[- ]run[- ]verify\\-agent\\-note\\-format|doc\\-sync|agent\\-note\\-format:[- ]alternatives\\-not\\-recorded)"
---

# 0387. One gated in-file format for Agent Notes — implementation context

## Open this when

Agent Note paths encoded lifecycle and class, but file contents still mixed headings, status formats, ADR and proposal templates, and proposal-era sections in implemented records. Authors copied whichever neighbor they found, and lifecycle moves could skip the required rewrite because no gate enforced an in-file contract.

## Source decision

README.md § The file format is the in-file contract --- the header block (# Agent Note: plus a dateless, folder-agreeing Status: enum whose only content is the rejection reason), the per-lifecycle body skeleton (Problem opener everywhere; Proposal/Acceptance criteria/Risks in proposed/; present-tense Decision/Consequences with proposal-era headings banned in implemented/; frozen proposal shape in rejected/), a mandatory Alternatives considered section, and the canonical section vocabulary between which bespoke technical sections stay free-form. pnpm run verify-agent-note-format (scripts/verify-agent-note-format.

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/process/2026-07-05-uniform-agent-note-format.md](../02-notes/implemented/process/2026-07-05-uniform-agent-note-format.md)
- Pinned source: [.agents/notes/implemented/process/2026-07-05-uniform-agent-note-format.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/process/2026-07-05-uniform-agent-note-format.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`scripts/verify-agent-note-format.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/verify-agent-note-format.ts) | repository automation | The source note names this file directly. | `named-file` |
| [`vendor/hmr/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/hmr/src/index.ts) | package entry point | Defines `Context`, a construct named by the note. | `symbol-definition` |
| [`vendor/timer/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/timer/src/index.ts) | package entry point | Defines `Context`, a construct named by the note. | `symbol-definition` |
| [`vendor/cordis/src/fiber.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/cordis/src/fiber.ts) | runtime implementation | Defines `Context`, a construct named by the note. | `symbol-definition` |
| [`vendor/loader/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/loader/src/index.ts) | package entry point | Defines `Context`, a construct named by the note. | `symbol-definition` |
| [`vendor/cordis/src/events.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/cordis/src/events.ts) | runtime implementation | Defines `Context`, a construct named by the note. | `symbol-definition` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `Context` | `interface` | [`vendor/cordis/src/events.ts:35`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/cordis/src/events.ts#L35) | `export interface Context {` |
| `Context` | `interface` | [`vendor/cordis/src/fiber.ts:10`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/cordis/src/fiber.ts#L10) | `export interface Context extends Pick<Fiber, 'effect'> {` |
| `Context` | `interface` | [`vendor/hmr/src/index.ts:16`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/hmr/src/index.ts#L16) | `interface Context {` |
| `Context` | `interface` | [`vendor/loader/src/index.ts:32`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/loader/src/index.ts#L32) | `interface Context {` |
| `Context` | `interface` | [`vendor/timer/src/index.ts:4`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/timer/src/index.ts#L4) | `interface Context extends Pick<TimerService, 'interval' \| 'timeout' \| 'throttle' \| 'debounce' \| 'setTimeout' \| 'setInterval'> {` |

### Tests and executable evidence

- [`scripts/run-gates.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/run-gates.spec.ts) — A test under the owning area exercises or imports `doc-sync`.
- [`scripts/archived-agent-notes.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/archived-agent-notes.spec.ts) — A test under the owning area exercises or imports `Problem`.
- [`scripts/gen-third-party-notices.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/gen-third-party-notices.spec.ts) — A test under the owning area exercises or imports `doc-sync`.

## How to read the implementation

1. Start with [`scripts/verify-agent-note-format.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/verify-agent-note-format.ts) because it has the strongest evidence link to the note.
2. Read public types and entry points before implementation details. Write down the contract in plain language.
3. Follow the named constructs into their callers and tests. Confirm which component can actually enforce the rule.
4. Reconstruct one failure path: cancellation, crash, retry, stale state, partial write, or incompatible provider input—whichever the note is protecting against.
5. Only then map the contract into the current Rust repository. Target paths must come from that repository, never from guesswork in this wiki.

## Rust harness port

**Likely integration point:** repository tests and engineering policy.

Preserve the behavior and ownership rule. Do not mechanically translate Cordis, TypeScript declaration merging, Node streams, or package topology into Rust.

Relevant standing rules:

- **`rust/append-log`** — Keep durable facts append-only and derive views from them. Separate the synchronous fact boundary from asynchronous persistence.
- **`rust/four-tool-surface`** — Preserve the four model tools: read, write, patch, and exec. Reuse internal capabilities without automatically exposing new model-facing tools.
- **`rust/process-discipline`** — Treat this as repository or release policy unless it changes a runtime contract. Do not add runtime machinery for a process-only lesson.
- **`rust/trait-seam`** — Use a small trait at the capability boundary; keep policy and model-facing schemas outside the backend implementation.

### Smallest safe implementation slice

1. Express the semantic contract as Rust types and one owning boundary.
2. Add the smallest adapter or service needed by the existing loop; do not add a new model-facing tool unless the four-tool surface cannot express the job.
3. Record durable evidence before deriving summaries or UI state.
4. Add one happy-path test, one interruption test, and one replay/restart test where state survives a turn.
5. Keep the patch reversible until the source note's strongest failure case is proven in the Rust harness.

## Search handles

- Tags: `class/process`, `concern/boundary`, `concern/evidence`, `concern/lifecycle`, `concern/ownership`, `concern/performance`, `domain/build-release`, `domain/session-state`, `domain/testing`, `lifecycle/implemented`, `mechanism/policy`
- Aliases: `Context`, `# Agent Note: <title>`, `proposed/`, `implemented/`, `rejected/`, `pnpm run verify-agent-note-format`, `doc-sync`, `agent-note-format: alternatives-not-recorded`, `Agent Note:`, `## What we give up`, `FORMAT.md`, `One gated in-file format for Agent Notes`, `process`, `boundary`
- Regex: `(?i)(Context|\#[- ]Agent[- ]Note:[- ]<title>|proposed/|implemented/|rejected/|pnpm[- ]run[- ]verify\-agent\-note\-format|doc\-sync|agent\-note\-format:[- ]alternatives\-not\-recorded)`

```bash
rg -n --pcre2 "(?i)(Context|\\#[- ]Agent[- ]Note:[- ]<title>|proposed/|implemented/|rejected/|pnpm[- ]run[- ]verify\\-agent\\-note\\-format|doc\\-sync|agent\\-note\\-format:[- ]alternatives\\-not\\-recorded)" source-deepseek-harness
rg -l --fixed-strings "class/process" 03-agent-context
```

## Connected notes

- **`source-link`** — [0687. Agent Notes](0687-agent-notes.md): The source note links to this decision directly.
- **`shares-code-with`** — [0103. Slot declaration injection and reload lifetimes](0103-slot-declaration-injection-and-reload-lifetimes.md): Shares source implementation: `vendor/cordis/src/events.ts`, `vendor/cordis/src/fiber.ts`.
- **`shares-code-with`** — [0052. The slot system standard --- single register, four props shares, and the framework store seat](0052-the-slot-system-standard-single-register-four-props-shares-and-the-frame.md): Shares source implementation: `vendor/cordis/src/events.ts`, `vendor/cordis/src/fiber.ts`.
- **`shares-code-with`** — [0439. Private npm publication as three independent sequences](0439-private-npm-publication-as-three-independent-sequences.md): Shares source implementation: `vendor/hmr/src/index.ts`, `vendor/loader/src/index.ts`.
- **`shares-code-with`** — [0546. Extract example apps into packages](0546-extract-example-apps-into-packages.md): Shares source implementation: `vendor/hmr/src/index.ts`, `vendor/loader/src/index.ts`.
- **`shares-code-with`** — [0388. Export JSDoc gate](0388-export-jsdoc-gate.md): Shares source implementation: `vendor/cordis/src/events.ts`, `vendor/hmr/src/index.ts`.
- **`shares-code-with`** — [0392. Package Model Experience contract](0392-package-model-experience-contract.md): Shares source implementation: `scripts/gen-third-party-notices.spec.ts`, `scripts/run-gates.spec.ts`.
- **`shares-code-with`** — [0044. GUI layering and the RPC protocol --- host/client layering by capability provider, the four-quadrant message model, and the fetch carrier](0044-gui-layering-and-the-rpc-protocol-host-client-layering-by-capability-pro.md): Shares source implementation: `vendor/hmr/src/index.ts`, `vendor/loader/src/index.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0387-one-gated-in-file-format-for-agent-notes.md`.
