---
id: "dsh-note-0645"
title: "GUI pull request GIF evidence and assets-branch publication"
status: "archived"
class: "process"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/archived/process/2026-07-26-gui-pr-gif-evidence-and-assets-branch.md"
implementation_evidence: "lead-only"
target_anchor: "repository tests and engineering policy"
tags:
  - "class/process"
  - "concern/boundary"
  - "concern/evidence"
  - "concern/lifecycle"
  - "concern/ownership"
  - "domain/build-release"
  - "domain/filesystem"
  - "domain/jobs-tasks"
  - "domain/llm"
  - "domain/protocols"
  - "domain/session-state"
  - "domain/testing"
  - "domain/web-retrieval"
  - "lifecycle/archived"
  - "mechanism/event-sourcing"
  - "mechanism/policy"
  - "mechanism/projection"
aliases:
  - "code-mode-ui-assets"
  - "pr-613-assets"
  - ".playwright-mcp/"
  - ".gitignore"
  - "GIF_SKILL_DIR"
  - "user-attachments"
  - "GUI pull request GIF evidence and assets-branch publication"
  - "process"
  - "boundary"
  - "evidence"
  - "lifecycle"
  - "ownership"
  - "build release"
  - "filesystem"
search_regex: "(?i)(code\\-mode\\-ui\\-assets|pr\\-613\\-assets|\\.playwright\\-mcp/|\\.gitignore|GIF_SKILL_DIR|user\\-attachments|GUI[- ]pull[- ]request[- ]GIF[- ]evidence[- ]and[- ]assets\\-branch[- ]publication|boundary)"
---

# 0645. GUI pull request GIF evidence and assets-branch publication — implementation context

## Open this when

A pull request that changes what a product user sees in the GUI is otherwise reviewed through prose and test names, neither of which shows the rendered result. The browser-demo GIF recording skill produces truthful local GIFs but deliberately stopped at the local artifact, so each pull request that wanted to show one re-derived publication on its own --- and committing the GIF to the pull request branch is never acceptable, because binary media in history bloats every future clone permanently.

## Source decision

Every pull request that changes product-user-visible GUI behavior includes a demonstration GIF recorded with the record-browser-gif skill, with real provenance --- a real server booted from that pull request's own branch tree, a real API key, and real model rounds --- stated next to the embed. Fixture provenance is acceptable only when the user explicitly asked for it. The GIF is published to a dedicated orphan assets branch --- no parent commit, media only --- never to the pull request branch; one assets branch serves a whole pull request series (existing branches: code-mode-ui-assets, pr-613-assets).

## Decision status

Historical design. Use it to understand an older pressure or failure; locate the replacing decision before implementing anything.

- Raw note: [archived/process/2026-07-26-gui-pr-gif-evidence-and-assets-branch.md](../02-notes/archived/process/2026-07-26-gui-pr-gif-evidence-and-assets-branch.md)
- Pinned source: [.agents/notes/archived/process/2026-07-26-gui-pr-gif-evidence-and-assets-branch.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/archived/process/2026-07-26-gui-pr-gif-evidence-and-assets-branch.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`.agents/skills/record-browser-gif/SKILL.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/skills/record-browser-gif/SKILL.md) | package contract and examples | The source note names this file directly. | `named-file` |

### Code constructs named by the note

The note names behavior or policy but no uniquely resolvable code declaration. Search by its aliases and inspect the listed package entry points.

### Tests and executable evidence

- No test file was tied to this note with enough confidence. Read the note's verification section and search the owning package's `tests/` directory.

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

- **`rust/append-log`** — Keep durable facts append-only and derive views from them. Separate the synchronous fact boundary from asynchronous persistence.
- **`rust/capability-security`** — Make granted capabilities explicit. On the Windows no-admin path, probe support at runtime and fail closed rather than silently running unsandboxed.
- **`rust/four-tool-surface`** — Preserve the four model tools: read, write, patch, and exec. Reuse internal capabilities without automatically exposing new model-facing tools.
- **`rust/process-discipline`** — Treat this as repository or release policy unless it changes a runtime contract. Do not add runtime machinery for a process-only lesson.
- **`rust/provider-adapter`** — Keep provider wire quirks inside adapters; the loop and durable event vocabulary remain provider-neutral.
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

- Tags: `class/process`, `concern/boundary`, `concern/evidence`, `concern/lifecycle`, `concern/ownership`, `domain/build-release`, `domain/filesystem`, `domain/jobs-tasks`, `domain/llm`, `domain/protocols`, `domain/session-state`, `domain/testing`, `domain/web-retrieval`, `lifecycle/archived`, `mechanism/event-sourcing`, `mechanism/policy`, `mechanism/projection`
- Aliases: `code-mode-ui-assets`, `pr-613-assets`, `.playwright-mcp/`, `.gitignore`, `GIF_SKILL_DIR`, `user-attachments`, `GUI pull request GIF evidence and assets-branch publication`, `process`, `boundary`, `evidence`, `lifecycle`, `ownership`, `build release`, `filesystem`
- Regex: `(?i)(code\-mode\-ui\-assets|pr\-613\-assets|\.playwright\-mcp/|\.gitignore|GIF_SKILL_DIR|user\-attachments|GUI[- ]pull[- ]request[- ]GIF[- ]evidence[- ]and[- ]assets\-branch[- ]publication|boundary)`

```bash
rg -n --pcre2 "(?i)(code\\-mode\\-ui\\-assets|pr\\-613\\-assets|\\.playwright\\-mcp/|\\.gitignore|GIF_SKILL_DIR|user\\-attachments|GUI[- ]pull[- ]request[- ]GIF[- ]evidence[- ]and[- ]assets\\-branch[- ]publication|boundary)" source-deepseek-harness
rg -l --fixed-strings "class/process" 03-agent-context
```

## Connected notes

- **`same-design-pressure`** — [0500. Keyless browser e2e lane for the web GUI](0500-keyless-browser-e2e-lane-for-the-web-gui.md): Shares design concerns: `concern/boundary`, `concern/evidence`, `concern/lifecycle`.
- **`same-design-pressure`** — [0044. GUI layering and the RPC protocol --- host/client layering by capability provider, the four-quadrant message model, and the fetch carrier](0044-gui-layering-and-the-rpc-protocol-host-client-layering-by-capability-pro.md): Shares design concerns: `concern/boundary`, `concern/evidence`, `concern/lifecycle`.
- **`same-design-pressure`** — [0542. Dependency swaps rejected by the 2026-07 NIH audit](0542-dependency-swaps-rejected-by-the-2026-07-nih-audit.md): Shares design concerns: `concern/boundary`, `concern/evidence`, `concern/lifecycle`.
- **`shares-code-with`** — [0429. Browser GIFs preserve one evidence chain](0429-browser-gifs-preserve-one-evidence-chain.md): Shares source implementation: `.agents/skills/record-browser-gif/SKILL.md`.
- **`same-design-pressure`** — [0398. GUI testing system --- the three-tier structure](0398-gui-testing-system-the-three-tier-structure.md): Shares design concerns: `concern/boundary`, `concern/evidence`, `concern/lifecycle`.
- **`same-design-pressure`** — [0016. Bounded recovery for transient LLM request failures](0016-bounded-recovery-for-transient-llm-request-failures.md): Shares design concerns: `concern/boundary`, `concern/evidence`, `concern/lifecycle`.
- **`shares-code-with`** — [0643. Browser demo GIF recording](0643-browser-demo-gif-recording.md): Shares source implementation: `.agents/skills/record-browser-gif/SKILL.md`.
- **`same-design-pressure`** — [0399. Serial cross-platform CI reference](0399-serial-cross-platform-ci-reference.md): Shares design concerns: `concern/boundary`, `concern/evidence`, `concern/lifecycle`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0645-gui-pull-request-gif-evidence-and-assets-branch-publication.md`.
