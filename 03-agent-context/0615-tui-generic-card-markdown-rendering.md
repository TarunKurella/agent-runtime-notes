---
id: "dsh-note-0615"
title: "TUI generic-card Markdown rendering"
status: "archived"
class: "bug-fix"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/archived/bug-fix/2026-07-23-tui-generic-card-markdown.md"
implementation_evidence: "lead-only"
target_anchor: "exec, terminal, and process lifecycle"
tags:
  - "class/bug-fix"
  - "concern/evidence"
  - "domain/build-release"
  - "domain/shell-terminal"
  - "domain/storage"
  - "domain/testing"
  - "domain/ui-interaction"
  - "lifecycle/archived"
aliases:
  - "TUI generic-card Markdown rendering"
  - "bug fix"
  - "evidence"
  - "build release"
  - "shell terminal"
  - "storage"
  - "testing"
  - "ui interaction"
  - "archived"
search_regex: "(?i)(TUI[- ]generic\\-card[- ]Markdown[- ]rendering|bug[- ]fix|evidence|build[- ]release|shell[- ]terminal|storage|testing|ui[- ]interaction)"
---

# 0615. TUI generic-card Markdown rendering — implementation context

## Open this when

Tool presenters can put Markdown in generic-card content, including fenced console output used for background-task acknowledgements and execution errors. Rendering that content as plain text exposes the fence markers and diverges from assistant and user content in the same transcript.

## Source decision

The TUI renders generic-card result content with its shared Markdown theme before applying the card's head-and-tail line limit. Terminal and diff cards retain their specialized plain-text renderers, and generic-card raw input remains literal because it represents tool arguments rather than presenter-authored prose. The shared theme hides fence syntax, retains the optional language label, and colors the fenced body as code. Rendering precedes truncation so collapsed-card line counts and boundaries describe the visible terminal rows rather than Markdown source rows.

## Decision status

Historical design. Use it to understand an older pressure or failure; locate the replacing decision before implementing anything.

- Raw note: [archived/bug-fix/2026-07-23-tui-generic-card-markdown.md](../02-notes/archived/bug-fix/2026-07-23-tui-generic-card-markdown.md)
- Pinned source: [.agents/notes/archived/bug-fix/2026-07-23-tui-generic-card-markdown.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/archived/bug-fix/2026-07-23-tui-generic-card-markdown.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`scripts/markdown.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/markdown.ts) | repository automation | Path shares title concepts: markdown. | `title-path-lead` |
| [`scripts/paired-markdown-derivatives.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/paired-markdown-derivatives.ts) | repository automation | Path shares title concepts: markdown. | `title-path-lead` |
| [`packages/client/ui-primitives/src/markdown/parse.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-primitives/src/markdown/parse.ts) | runtime implementation | Path shares title concepts: markdown. | `title-path-lead` |
| [`packages/client/ui-primitives/src/markdown/katex.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-primitives/src/markdown/katex.tsx) | runtime implementation | Path shares title concepts: markdown. | `title-path-lead` |
| [`packages/client/ui-primitives/src/markdown/render.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-primitives/src/markdown/render.tsx) | runtime implementation | Path shares title concepts: markdown. | `title-path-lead` |
| [`packages/client/ui-primitives/src/markdown/highlight.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-primitives/src/markdown/highlight.ts) | runtime implementation | Path shares title concepts: markdown. | `title-path-lead` |
| [`packages/client/ui-primitives/src/markdown/CodeBlock.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-primitives/src/markdown/CodeBlock.tsx) | runtime implementation | Path shares title concepts: markdown. | `title-path-lead` |
| [`packages/client/ui-primitives/src/markdown/JsonBlock.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-primitives/src/markdown/JsonBlock.tsx) | runtime implementation | Path shares title concepts: markdown. | `title-path-lead` |
| [`packages/client/ui-primitives/src/markdown/plain-text.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-primitives/src/markdown/plain-text.ts) | runtime implementation | Path shares title concepts: markdown. | `title-path-lead` |
| [`packages/client/ui-primitives/src/markdown/incremental.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-primitives/src/markdown/incremental.ts) | runtime implementation | Path shares title concepts: markdown. | `title-path-lead` |
| [`packages/client/ui-primitives/src/markdown/MessageText.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-primitives/src/markdown/MessageText.tsx) | runtime implementation | Path shares title concepts: markdown. | `title-path-lead` |
| [`packages/client/ui-primitives/src/markdown/MarkdownText.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-primitives/src/markdown/MarkdownText.tsx) | runtime implementation | Path shares title concepts: markdown. | `title-path-lead` |

### Code constructs named by the note

The note names behavior or policy but no uniquely resolvable code declaration. Search by its aliases and inspect the listed package entry points.

### Tests and executable evidence

- [`apps/web/tests/math-rendering.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/math-rendering.e2e.ts) — Path shares title concepts: rendering.
- [`apps/web/tests/markdown-images.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/markdown-images.e2e.ts) — Path shares title concepts: markdown.
- [`apps/web/tests/markdown-cjk-strong.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/markdown-cjk-strong.e2e.ts) — Path shares title concepts: markdown.
- [`scripts/paired-markdown-derivatives.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/paired-markdown-derivatives.spec.ts) — Path shares title concepts: markdown.
- [`apps/web/tests/markdown-inline-code-links.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/markdown-inline-code-links.e2e.ts) — Path shares title concepts: markdown.
- [`apps/web/tests/snapshots/math-rendering/ui.expected.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/snapshots/math-rendering/ui.expected.md) — Path shares title concepts: rendering.
- [`apps/web/tests/snapshots/markdown-images/ui.expected.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/snapshots/markdown-images/ui.expected.md) — Path shares title concepts: markdown.
- [`apps/web/tests/snapshots/markdown-cjk-strong/ui.expected.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/snapshots/markdown-cjk-strong/ui.expected.md) — Path shares title concepts: markdown.

## How to read the implementation

1. Start with [`scripts/markdown.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/markdown.ts) because it has the strongest evidence link to the note.
2. Read public types and entry points before implementation details. Write down the contract in plain language.
3. Follow the named constructs into their callers and tests. Confirm which component can actually enforce the rule.
4. Reconstruct one failure path: cancellation, crash, retry, stale state, partial write, or incompatible provider input—whichever the note is protecting against.
5. Only then map the contract into the current Rust repository. Target paths must come from that repository, never from guesswork in this wiki.

## Rust harness port

**Likely integration point:** exec, terminal, and process lifecycle.

Preserve the behavior and ownership rule. Do not mechanically translate Cordis, TypeScript declaration merging, Node streams, or package topology into Rust.

Relevant standing rules:

- **`rust/append-log`** — Keep durable facts append-only and derive views from them. Separate the synchronous fact boundary from asynchronous persistence.
- **`rust/four-tool-surface`** — Preserve the four model tools: read, write, patch, and exec. Reuse internal capabilities without automatically exposing new model-facing tools.
- **`rust/process-discipline`** — Treat this as repository or release policy unless it changes a runtime contract. Do not add runtime machinery for a process-only lesson.
- **`rust/tokio-cancellation`** — Thread one cancellation token through async work, close owned resources before reuse, and test cancellation at every await boundary.
- **Status gate** — Historical design. Use it to understand an older pressure or failure; locate the replacing decision before implementing anything.

### Smallest safe implementation slice

1. Express the semantic contract as Rust types and one owning boundary.
2. Add the smallest adapter or service needed by the existing loop; do not add a new model-facing tool unless the four-tool surface cannot express the job.
3. Record durable evidence before deriving summaries or UI state.
4. Add one happy-path test, one interruption test, and one replay/restart test where state survives a turn.
5. Keep the patch reversible until the source note's strongest failure case is proven in the Rust harness.

## Search handles

- Tags: `class/bug-fix`, `concern/evidence`, `domain/build-release`, `domain/shell-terminal`, `domain/storage`, `domain/testing`, `domain/ui-interaction`, `lifecycle/archived`
- Aliases: `TUI generic-card Markdown rendering`, `bug fix`, `evidence`, `build release`, `shell terminal`, `storage`, `testing`, `ui interaction`, `archived`
- Regex: `(?i)(TUI[- ]generic\-card[- ]Markdown[- ]rendering|bug[- ]fix|evidence|build[- ]release|shell[- ]terminal|storage|testing|ui[- ]interaction)`

```bash
rg -n --pcre2 "(?i)(TUI[- ]generic\\-card[- ]Markdown[- ]rendering|bug[- ]fix|evidence|build[- ]release|shell[- ]terminal|storage|testing|ui[- ]interaction)" source-deepseek-harness
rg -l --fixed-strings "class/bug-fix" 03-agent-context
```

## Connected notes

- **`same-design-pressure`** — [0604. Versioned TUI first-run welcome](0604-versioned-tui-first-run-welcome.md): Shares design concerns: `concern/evidence`, `domain/build-release`, `domain/shell-terminal`.
- **`same-design-pressure`** — [0579. Product-level TUI session resume](0579-product-level-tui-session-resume.md): Shares design concerns: `concern/evidence`, `domain/build-release`, `domain/shell-terminal`.
- **`same-design-pressure`** — [0573. TUI banner brand gradient](0573-tui-banner-brand-gradient.md): Shares design concerns: `concern/evidence`, `domain/build-release`, `domain/shell-terminal`.
- **`same-design-pressure`** — [0568. Startup slogans replace the configured TUI welcome line](0568-startup-slogans-replace-the-configured-tui-welcome-line.md): Shares design concerns: `concern/evidence`, `domain/build-release`, `domain/shell-terminal`.
- **`same-design-pressure`** — [0567. Dedicated full-screen TUI front door](0567-dedicated-full-screen-tui-front-door.md): Shares design concerns: `concern/evidence`, `domain/build-release`, `domain/shell-terminal`.
- **`same-design-pressure`** — [0668. Ship the TUI without `todo_write`; keep it a one-line opt-in](0668-ship-the-tui-without-todo-write-keep-it-a-one-line-opt-in.md): Shares design concerns: `concern/evidence`, `domain/build-release`, `domain/shell-terminal`.
- **`same-design-pressure`** — [0669. TUI titles come from the session-title service](0669-tui-titles-come-from-the-session-title-service.md): Shares design concerns: `concern/evidence`, `domain/build-release`, `domain/shell-terminal`.
- **`same-design-pressure`** — [0683. Snapshot semantic terminal state for the TUI](0683-snapshot-semantic-terminal-state-for-the-tui.md): Shares design concerns: `concern/evidence`, `domain/build-release`, `domain/shell-terminal`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0615-tui-generic-card-markdown-rendering.md`.
