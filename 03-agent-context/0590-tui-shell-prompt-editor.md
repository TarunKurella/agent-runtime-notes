---
id: "dsh-note-0590"
title: "TUI shell-prompt editor"
status: "archived"
class: "feature"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/archived/feature/2026-07-24-tui-shell-prompt-editor.md"
implementation_evidence: "lead-only"
target_anchor: "read/write/patch filesystem capability"
tags:
  - "class/feature"
  - "concern/boundary"
  - "concern/cancellation-timeout"
  - "concern/evidence"
  - "concern/performance"
  - "domain/build-release"
  - "domain/filesystem"
  - "domain/llm"
  - "domain/session-state"
  - "domain/shell-terminal"
  - "domain/ui-interaction"
  - "lifecycle/archived"
aliases:
  - "@earendil-works/pi-tui"
  - "EditorOptions"
  - "/status"
  - "↑ N more"
  - "↓ N more"
  - "TUI shell-prompt editor"
  - "feature"
  - "boundary"
  - "cancellation timeout"
  - "evidence"
  - "performance"
  - "build release"
  - "filesystem"
  - "llm"
search_regex: "(?i)(@earendil\\-works/pi\\-tui|EditorOptions|/status|↑[- ]N[- ]more|↓[- ]N[- ]more|TUI[- ]shell\\-prompt[- ]editor|feature|boundary)"
---

# 0590. TUI shell-prompt editor — implementation context

## Open this when

The upstream pi-tui editor always renders horizontal frame rows. That presentation separates input from the transcript but occupies two terminal rows and does not resemble the command-oriented input used by shells.

## Source decision

The TUI presents a two-line prompt. A DSH-owned context line shows the working directory, running-turn timing, optional Git branch, current model, token totals, cache hit rate, and context pressure as independently prioritized segments. Narrow terminals omit lower-priority segments while retaining the directory, followed by running timing when it is present. The second line uses a fixed-width dsh> prefix and equal-width continuation indent; its running steer/cancel guidance is placeholder text that disappears when input begins.

## Decision status

Historical design. Use it to understand an older pressure or failure; locate the replacing decision before implementing anything.

- Raw note: [archived/feature/2026-07-24-tui-shell-prompt-editor.md](../02-notes/archived/feature/2026-07-24-tui-shell-prompt-editor.md)
- Pinned source: [.agents/notes/archived/feature/2026-07-24-tui-shell-prompt-editor.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/archived/feature/2026-07-24-tui-shell-prompt-editor.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`packages/fs/tool-str-replace-editor/README.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/tool-str-replace-editor/README.md) | package contract and examples | Path shares title concepts: editor. | `title-path-lead` |
| [`packages/fs/tool-str-replace-editor/README.zh.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/tool-str-replace-editor/README.zh.md) | package contract and examples | Path shares title concepts: editor. | `title-path-lead` |
| [`packages/fs/tool-str-replace-editor/package.json`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/tool-str-replace-editor/package.json) | composition and configuration | Path shares title concepts: editor. | `title-path-lead` |
| [`packages/fs/tool-str-replace-editor/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/tool-str-replace-editor/src/index.ts) | package entry point | Path shares title concepts: editor. | `title-path-lead` |
| [`packages/fs/tool-str-replace-editor/tsconfig.json`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/tool-str-replace-editor/tsconfig.json) | composition and configuration | Path shares title concepts: editor. | `title-path-lead` |
| [`packages/fs/tool-str-replace-editor/README.i18n.yaml`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/tool-str-replace-editor/README.i18n.yaml) | composition and configuration | Path shares title concepts: editor. | `title-path-lead` |
| [`packages/fs/tool-str-replace-editor/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/tool-str-replace-editor/src/invariant.ts) | runtime contract checks | Path shares title concepts: editor. | `title-path-lead` |

### Code constructs named by the note

The note names behavior or policy but no uniquely resolvable code declaration. Search by its aliases and inspect the listed package entry points.

### Tests and executable evidence

- [`packages/fs/tool-str-replace-editor/tests/tools.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/tool-str-replace-editor/tests/tools.spec.ts) — Path shares title concepts: editor.

## How to read the implementation

1. Start with [`packages/fs/tool-str-replace-editor/README.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/tool-str-replace-editor/README.md) because it has the strongest evidence link to the note.
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

- Tags: `class/feature`, `concern/boundary`, `concern/cancellation-timeout`, `concern/evidence`, `concern/performance`, `domain/build-release`, `domain/filesystem`, `domain/llm`, `domain/session-state`, `domain/shell-terminal`, `domain/ui-interaction`, `lifecycle/archived`
- Aliases: `@earendil-works/pi-tui`, `EditorOptions`, `/status`, `↑ N more`, `↓ N more`, `TUI shell-prompt editor`, `feature`, `boundary`, `cancellation timeout`, `evidence`, `performance`, `build release`, `filesystem`, `llm`
- Regex: `(?i)(@earendil\-works/pi\-tui|EditorOptions|/status|↑[- ]N[- ]more|↓[- ]N[- ]more|TUI[- ]shell\-prompt[- ]editor|feature|boundary)`

```bash
rg -n --pcre2 "(?i)(@earendil\\-works/pi\\-tui|EditorOptions|/status|\u2191[- ]N[- ]more|\u2193[- ]N[- ]more|TUI[- ]shell\\-prompt[- ]editor|feature|boundary)" source-deepseek-harness
rg -l --fixed-strings "class/feature" 03-agent-context
```

## Connected notes

- **`same-design-pressure`** — [0585. TUI file-reference autocomplete](0585-tui-file-reference-autocomplete.md): Shares design concerns: `concern/boundary`, `concern/cancellation-timeout`, `concern/evidence`.
- **`same-design-pressure`** — [0579. Product-level TUI session resume](0579-product-level-tui-session-resume.md): Shares design concerns: `concern/boundary`, `concern/cancellation-timeout`, `concern/evidence`.
- **`same-design-pressure`** — [0567. Dedicated full-screen TUI front door](0567-dedicated-full-screen-tui-front-door.md): Shares design concerns: `concern/boundary`, `concern/cancellation-timeout`, `concern/evidence`.
- **`same-design-pressure`** — [0669. TUI titles come from the session-title service](0669-tui-titles-come-from-the-session-title-service.md): Shares design concerns: `concern/boundary`, `concern/cancellation-timeout`, `concern/evidence`.
- **`same-design-pressure`** — [0564. Bash-backed grep and glob discovery tools](0564-bash-backed-grep-and-glob-discovery-tools.md): Shares design concerns: `concern/boundary`, `concern/cancellation-timeout`, `concern/evidence`.
- **`same-design-pressure`** — [0557. Agent Client Protocol (ACP) support --- drive the coding agent from external editors](0557-agent-client-protocol-acp-support-drive-the-coding-agent-from-external-e.md): Shares design concerns: `concern/boundary`, `concern/cancellation-timeout`, `concern/evidence`.
- **`same-design-pressure`** — [0509. Domain KV storage capability seam and the workspace entity](0509-domain-kv-storage-capability-seam-and-the-workspace-entity.md): Shares design concerns: `concern/boundary`, `concern/cancellation-timeout`, `concern/evidence`.
- **`same-design-pressure`** — [0500. Keyless browser e2e lane for the web GUI](0500-keyless-browser-e2e-lane-for-the-web-gui.md): Shares design concerns: `concern/boundary`, `concern/cancellation-timeout`, `concern/evidence`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0590-tui-shell-prompt-editor.md`.
