---
id: "dsh-note-0643"
title: "Browser demo GIF recording"
status: "archived"
class: "process"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/archived/process/2026-07-23-browser-demo-gif-recording.md"
implementation_evidence: "lead-only"
target_anchor: "repository tests and engineering policy"
tags:
  - "class/process"
  - "concern/boundary"
  - "concern/evidence"
  - "concern/recovery"
  - "domain/build-release"
  - "domain/protocols"
  - "domain/testing"
  - "domain/web-retrieval"
  - "lifecycle/archived"
  - "mechanism/policy"
aliases:
  - "record-browser-gif"
  - ".playwright-mcp/"
  - "encode_gif.py"
  - "Browser demo GIF recording"
  - "process"
  - "boundary"
  - "evidence"
  - "recovery"
  - "build release"
  - "protocols"
  - "testing"
  - "web retrieval"
  - "archived"
  - "policy"
search_regex: "(?i)(record\\-browser\\-gif|\\.playwright\\-mcp/|encode_gif\\.py|Browser[- ]demo[- ]GIF[- ]recording|boundary|evidence|recovery|build[- ]release)"
---

# 0643. Browser demo GIF recording — implementation context

## Open this when

Browser demonstrations have been assembled with one-off capture and encoding commands. That makes timing and output size inconsistent, encourages continuous recordings that obscure the useful state changes, and can blur the boundary between a genuine server or API flow and a fixture. Combining local recording with attachment upload or pull-request editing also gives a media task unrelated remote-write authority.

## Source decision

The repository provides the record-browser-gif skill for local browser-demo artifacts. It uses the available browser-control workflow, establishes whether the requested flow is real, fixture-backed, or otherwise simulated, and captures a small storyboard only after semantically observable UI states. Frames live under the repository's gitignored .playwright-mcp/ directory --- the browser tool writes only under its allowed roots --- and never dirty the worktree.

## Decision status

Historical design. Use it to understand an older pressure or failure; locate the replacing decision before implementing anything.

- Raw note: [archived/process/2026-07-23-browser-demo-gif-recording.md](../02-notes/archived/process/2026-07-23-browser-demo-gif-recording.md)
- Pinned source: [.agents/notes/archived/process/2026-07-23-browser-demo-gif-recording.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/archived/process/2026-07-23-browser-demo-gif-recording.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`.agents/skills/record-browser-gif/SKILL.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/skills/record-browser-gif/SKILL.md) | package contract and examples | The source note names this file directly. | `named-file` |

### Code constructs named by the note

The note names behavior or policy but no uniquely resolvable code declaration. Search by its aliases and inspect the listed package entry points.

### Tests and executable evidence

- [`apps/web/tests/snapshots/permission-policy-context/session.jsonl`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/snapshots/permission-policy-context/session.jsonl) — A test under the owning area exercises or imports `ffmpeg`.

## How to read the implementation

1. Start with [`.agents/skills/record-browser-gif/SKILL.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/skills/record-browser-gif/SKILL.md) because it has the strongest evidence link to the note.
2. Read public types and entry points before implementation details. Write down the contract in plain language.
3. Follow the named constructs into their callers and tests. Confirm which component can actually enforce the rule.
4. Reconstruct one failure path: cancellation, crash, retry, stale state, partial write, or incompatible provider input—whichever the note is protecting against.
5. Only then map the contract into the current Rust repository. Target paths must come from that repository, never from guesswork in this wiki.

## Rust harness port

**Likely integration point:** repository tests and engineering policy.

Preserve the behavior and ownership rule. Do not mechanically translate Cordis, TypeScript declaration merging, Node streams, or package topology into Rust.

Relevant standing rules:

- **`rust/four-tool-surface`** — Preserve the four model tools: read, write, patch, and exec. Reuse internal capabilities without automatically exposing new model-facing tools.
- **`rust/process-discipline`** — Treat this as repository or release policy unless it changes a runtime contract. Do not add runtime machinery for a process-only lesson.
- **`rust/trait-seam`** — Use a small trait at the capability boundary; keep policy and model-facing schemas outside the backend implementation.
- **Status gate** — Historical design. Use it to understand an older pressure or failure; locate the replacing decision before implementing anything.

### Smallest safe implementation slice

1. Express the semantic contract as Rust types and one owning boundary.
2. Add the smallest adapter or service needed by the existing loop; do not add a new model-facing tool unless the four-tool surface cannot express the job.
3. Record durable evidence before deriving summaries or UI state.
4. Add one happy-path test, one interruption test, and one replay/restart test where state survives a turn.
5. Keep the patch reversible until the source note's strongest failure case is proven in the Rust harness.

## Search handles

- Tags: `class/process`, `concern/boundary`, `concern/evidence`, `concern/recovery`, `domain/build-release`, `domain/protocols`, `domain/testing`, `domain/web-retrieval`, `lifecycle/archived`, `mechanism/policy`
- Aliases: `record-browser-gif`, `.playwright-mcp/`, `encode_gif.py`, `Browser demo GIF recording`, `process`, `boundary`, `evidence`, `recovery`, `build release`, `protocols`, `testing`, `web retrieval`, `archived`, `policy`
- Regex: `(?i)(record\-browser\-gif|\.playwright\-mcp/|encode_gif\.py|Browser[- ]demo[- ]GIF[- ]recording|boundary|evidence|recovery|build[- ]release)`

```bash
rg -n --pcre2 "(?i)(record\\-browser\\-gif|\\.playwright\\-mcp/|encode_gif\\.py|Browser[- ]demo[- ]GIF[- ]recording|boundary|evidence|recovery|build[- ]release)" source-deepseek-harness
rg -l --fixed-strings "class/process" 03-agent-context
```

## Connected notes

- **`source-link`** — [0645. GUI pull request GIF evidence and assets-branch publication](0645-gui-pull-request-gif-evidence-and-assets-branch-publication.md): The source note links to this decision directly.
- **`shares-code-with`** — [0429. Browser GIFs preserve one evidence chain](0429-browser-gifs-preserve-one-evidence-chain.md): Shares source implementation: `.agents/skills/record-browser-gif/SKILL.md`.
- **`shares-code-with`** — [0425. The documentation site carries its own images](0425-the-documentation-site-carries-its-own-images.md): Shares source implementation: `apps/web/tests/snapshots/permission-policy-context/session.jsonl`.
- **`shares-code-with`** — [0390. Parallel pre-push gates](0390-parallel-pre-push-gates.md): Shares source implementation: `apps/web/tests/snapshots/permission-policy-context/session.jsonl`.
- **`shares-code-with`** — [0110. Bounded session persistence write batching](0110-bounded-session-persistence-write-batching.md): Shares source implementation: `apps/web/tests/snapshots/permission-policy-context/session.jsonl`.
- **`same-design-pressure`** — [0500. Keyless browser e2e lane for the web GUI](0500-keyless-browser-e2e-lane-for-the-web-gui.md): Shares design concerns: `concern/boundary`, `concern/evidence`, `concern/recovery`.
- **`same-design-pressure`** — [0099. WebSocket carrier for browser downlinks](0099-websocket-carrier-for-browser-downlinks.md): Shares design concerns: `concern/boundary`, `concern/evidence`, `concern/recovery`.
- **`shares-code-with`** — [0673. Copyable TUI transcript without gutter bars](0673-copyable-tui-transcript-without-gutter-bars.md): Shares source implementation: `apps/web/tests/snapshots/permission-policy-context/session.jsonl`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0643-browser-demo-gif-recording.md`.
