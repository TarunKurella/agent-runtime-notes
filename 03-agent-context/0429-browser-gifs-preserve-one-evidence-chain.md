---
id: "dsh-note-0429"
title: "Browser GIFs preserve one evidence chain"
status: "implemented"
class: "process"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/process/2026-08-08-browser-gif-evidence-chain.md"
implementation_evidence: "lead-only"
target_anchor: "repository tests and engineering policy"
tags:
  - "class/process"
  - "concern/boundary"
  - "concern/evidence"
  - "concern/recovery"
  - "concern/trust"
  - "domain/build-release"
  - "domain/configuration"
  - "domain/extensions"
  - "domain/filesystem"
  - "domain/llm"
  - "domain/observability"
  - "domain/session-state"
  - "domain/storage"
  - "domain/testing"
  - "domain/tools"
  - "domain/ui-interaction"
  - "domain/web-retrieval"
  - "lifecycle/implemented"
aliases:
  - "record-browser-gif"
  - "DSH_HOME"
  - "DSH_AGENTS_HOME"
  - "Browser GIFs preserve one evidence chain"
  - "process"
  - "boundary"
  - "evidence"
  - "recovery"
  - "trust"
  - "build release"
  - "configuration"
  - "extensions"
  - "filesystem"
  - "llm"
search_regex: "(?i)(record\\-browser\\-gif|DSH_HOME|DSH_AGENTS_HOME|Browser[- ]GIFs[- ]preserve[- ]one[- ]evidence[- ]chain|boundary|evidence|recovery|trust)"
---

# 0429. Browser GIFs preserve one evidence chain — implementation context

## Open this when

A browser-demo storyboard can contain individually truthful screenshots without proving one truthful execution. Reusing global application state can admit old settings or sessions, capture automation can accidentally combine frames from separate model runs, and a chat transcript can show a successful fallback without exposing the tool rejection that caused it. Fuzzy accessible-name matching can also accept prompt echoes or descendant text instead of the intended result. Headless production recording has two further boundaries.

## Source decision

The record-browser-gif workflow treats one storyboard as one evidence chain pinned to an exact pull-request head. Before building, it requires a clean worktree and records that commit SHA. Each run uses fresh DSH_HOME, DSH_AGENTS_HOME, workspace, session, and isolated browser state, and every published frame comes from the same server and model-backed scenario run. When a fresh browser context is unavailable, the exact origin's cookies and site storage are cleared before navigation.

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/process/2026-08-08-browser-gif-evidence-chain.md](../02-notes/implemented/process/2026-08-08-browser-gif-evidence-chain.md)
- Pinned source: [.agents/notes/implemented/process/2026-08-08-browser-gif-evidence-chain.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/process/2026-08-08-browser-gif-evidence-chain.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`.agents/skills/record-browser-gif/SKILL.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/skills/record-browser-gif/SKILL.md) | package contract and examples | The source note names this file directly. | `named-file` |

### Code constructs named by the note

The note names behavior or policy but no uniquely resolvable code declaration. Search by its aliases and inspect the listed package entry points.

### Tests and executable evidence

- [`apps/web/tests/scaffold.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/scaffold.ts) — A test under the owning area exercises or imports `DSH_HOME`. A test under the owning area exercises or imports `DSH_AGENTS_HOME`.
- [`apps/web/tests/hmr-live.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/hmr-live.e2e.ts) — A test under the owning area exercises or imports `DSH_HOME`.
- [`apps/cli/tests/built-bin.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/cli/tests/built-bin.e2e.ts) — A test under the owning area exercises or imports `DSH_HOME`.
- [`apps/web/tests/smoke-real.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/smoke-real.e2e.ts) — A test under the owning area exercises or imports `DSH_HOME`. A test under the owning area exercises or imports `DSH_AGENTS_HOME`.
- [`apps/web/tests/seeded-history.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/seeded-history.e2e.ts) — A test under the owning area exercises or imports `DSH_HOME`.
- [`apps/web/tests/scaffold-hermetic.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/scaffold-hermetic.e2e.ts) — A test under the owning area exercises or imports `DSH_HOME`. A test under the owning area exercises or imports `DSH_AGENTS_HOME`.
- [`apps/cli/tests/headless-shutdown.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/cli/tests/headless-shutdown.e2e.ts) — A test under the owning area exercises or imports `DSH_HOME`. A test under the owning area exercises or imports `DSH_AGENTS_HOME`.
- [`apps/cli/tests/web-agent-presets.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/cli/tests/web-agent-presets.e2e.ts) — A test under the owning area exercises or imports `DSH_HOME`.

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
- **`rust/quickjs-boundary`** — QuickJS holds orchestration variables; Rust/Tokio owns async execution and exposes only explicitly granted capabilities.
- **`rust/trait-seam`** — Use a small trait at the capability boundary; keep policy and model-facing schemas outside the backend implementation.

### Smallest safe implementation slice

1. Express the semantic contract as Rust types and one owning boundary.
2. Add the smallest adapter or service needed by the existing loop; do not add a new model-facing tool unless the four-tool surface cannot express the job.
3. Record durable evidence before deriving summaries or UI state.
4. Add one happy-path test, one interruption test, and one replay/restart test where state survives a turn.
5. Keep the patch reversible until the source note's strongest failure case is proven in the Rust harness.

## Search handles

- Tags: `class/process`, `concern/boundary`, `concern/evidence`, `concern/recovery`, `concern/trust`, `domain/build-release`, `domain/configuration`, `domain/extensions`, `domain/filesystem`, `domain/llm`, `domain/observability`, `domain/session-state`, `domain/storage`, `domain/testing`, `domain/tools`, `domain/ui-interaction`, `domain/web-retrieval`, `lifecycle/implemented`
- Aliases: `record-browser-gif`, `DSH_HOME`, `DSH_AGENTS_HOME`, `Browser GIFs preserve one evidence chain`, `process`, `boundary`, `evidence`, `recovery`, `trust`, `build release`, `configuration`, `extensions`, `filesystem`, `llm`
- Regex: `(?i)(record\-browser\-gif|DSH_HOME|DSH_AGENTS_HOME|Browser[- ]GIFs[- ]preserve[- ]one[- ]evidence[- ]chain|boundary|evidence|recovery|trust)`

```bash
rg -n --pcre2 "(?i)(record\\-browser\\-gif|DSH_HOME|DSH_AGENTS_HOME|Browser[- ]GIFs[- ]preserve[- ]one[- ]evidence[- ]chain|boundary|evidence|recovery|trust)" source-deepseek-harness
rg -l --fixed-strings "class/process" 03-agent-context
```

## Connected notes

- **`source-link`** — [0425. The documentation site carries its own images](0425-the-documentation-site-carries-its-own-images.md): The source note links to this decision directly.
- **`shares-code-with`** — [0604. Versioned TUI first-run welcome](0604-versioned-tui-first-run-welcome.md): Shares source implementation: `apps/cli/tests/built-bin.e2e.ts`, `apps/cli/tests/headless-shutdown.e2e.ts`.
- **`shares-code-with`** — [0684. Adopt execa for hand-rolled test subprocess plumbing](0684-adopt-execa-for-hand-rolled-test-subprocess-plumbing.md): Shares source implementation: `apps/cli/tests/built-bin.e2e.ts`, `apps/web/tests/scaffold.ts`.
- **`shares-code-with`** — [0456. Remove the stdio and Echo agents](0456-remove-the-stdio-and-echo-agents.md): Shares source implementation: `apps/cli/tests/built-bin.e2e.ts`, `apps/cli/tests/headless-shutdown.e2e.ts`.
- **`shares-code-with`** — [0611. Keep the Code Mode result card complete](0611-keep-the-code-mode-result-card-complete.md): Shares source implementation: `apps/web/tests/scaffold.ts`, `apps/web/tests/smoke-real.e2e.ts`.
- **`shares-code-with`** — [0500. Keyless browser e2e lane for the web GUI](0500-keyless-browser-e2e-lane-for-the-web-gui.md): Shares source implementation: `apps/web/tests/scaffold.ts`.
- **`shares-code-with`** — [0359. Pre-Plugin Theme Bootstrap](0359-pre-plugin-theme-bootstrap.md): Shares source implementation: `apps/web/tests/scaffold.ts`, `apps/web/tests/smoke-real.e2e.ts`.
- **`shares-code-with`** — [0296. Status-driven disclosure for workflow runs](0296-status-driven-disclosure-for-workflow-runs.md): Shares source implementation: `apps/web/tests/seeded-history.e2e.ts`, `apps/web/tests/smoke-real.e2e.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0429-browser-gifs-preserve-one-evidence-chain.md`.
