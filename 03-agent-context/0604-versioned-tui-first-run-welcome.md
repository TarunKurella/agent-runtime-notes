---
id: "dsh-note-0604"
title: "Versioned TUI first-run welcome"
status: "archived"
class: "feature"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/archived/feature/2026-07-30-versioned-tui-first-run-welcome.md"
implementation_evidence: "medium"
target_anchor: "Rust provider-adapter boundary"
tags:
  - "class/feature"
  - "concern/boundary"
  - "concern/compatibility"
  - "concern/concurrency"
  - "concern/discovery-routing"
  - "concern/evidence"
  - "concern/lifecycle"
  - "concern/ownership"
  - "concern/recovery"
  - "domain/build-release"
  - "domain/configuration"
  - "domain/extensions"
  - "domain/llm"
  - "domain/session-state"
  - "domain/shell-terminal"
  - "domain/storage"
  - "domain/testing"
  - "domain/ui-interaction"
  - "lifecycle/archived"
  - "mechanism/event-sourcing"
  - "mechanism/policy"
aliases:
  - "dsh"
  - "DSH_HOME"
  - "ctx.tui.openOverlay"
  - "fallback. ANSI styling stays outside both the SVG and editable copy:"
  - "Versioned TUI first-run welcome"
  - "feature"
  - "boundary"
  - "compatibility"
  - "concurrency"
  - "discovery routing"
  - "evidence"
  - "lifecycle"
  - "ownership"
  - "recovery"
search_regex: "(?i)(DSH_HOME|ctx\\.tui\\.openOverlay|Versioned[- ]TUI[- ]first\\-run[- ]welcome|feature|boundary|compatibility|concurrency|discovery[- ]routing)"
---

# 0604. Versioned TUI first-run welcome — implementation context

## Open this when

The shipped dsh terminal starts directly in the editor and gives first-time internal testers no durable orientation about the product's maturity or feedback channel. The existing one-line welcome banner subtitle cannot carry the supplied notice without crowding the normal session header, and putting onboarding in the session log would create a user turn or model-visible context that is unrelated to the user's work. The notice also needs a recognizable DeepSeek composition without copying another product's startup art or maintaining a hand-drawn approximation that drifts from the official mark.

## Source decision

The official dsh launcher owns one versioned acknowledgement marker under the resolved DSH_HOME. It checks the immutable marker before boot, then mounts an effect-owned consumer of ctx.tui.openOverlay() only after the real TUI service is available. Enter is the sole acknowledgement action: the plugin creates and synchronizes the fixed per-version marker before closing. Escape and unrecognized input leave the overlay open; Ctrl+C and Ctrl+D use the normal exit path without acknowledging. Disposal waits for an acknowledgement already started by Enter, while disposal or process exit before Enter writes nothing.

## Decision status

Historical design. Use it to understand an older pressure or failure; locate the replacing decision before implementing anything.

- Raw note: [archived/feature/2026-07-30-versioned-tui-first-run-welcome.md](../02-notes/archived/feature/2026-07-30-versioned-tui-first-run-welcome.md)
- Pinned source: [.agents/notes/archived/feature/2026-07-30-versioned-tui-first-run-welcome.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/archived/feature/2026-07-30-versioned-tui-first-run-welcome.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`apps/cli`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/apps/cli) | package or module directory | The note names this package or capability. | `named-package` |
| [`packages/client/modules/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/modules/src/index.ts) | package entry point | Defines `dsh`, a construct named by the note. | `symbol-definition` |
| [`apps/cli/README.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/cli/README.md) | package contract and examples | Core file in the package named by the note: `apps/cli`. | `named-package-member` |
| [`apps/cli/package.json`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/cli/package.json) | composition and configuration | Core file in the package named by the note: `apps/cli`. | `named-package-member` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `dsh` | `const` | [`packages/client/modules/src/index.ts:345`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/modules/src/index.ts#L345) | `const dsh = pkg.dsh` |

### Tests and executable evidence

- [`apps/cli/tests/built-bin.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/cli/tests/built-bin.e2e.ts) — A test under the owning area exercises or imports `DSH_HOME`.
- [`apps/cli/tests/headless-shutdown.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/cli/tests/headless-shutdown.e2e.ts) — A test under the owning area exercises or imports `DSH_HOME`.
- [`apps/cli/tests/web-agent-presets.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/cli/tests/web-agent-presets.e2e.ts) — A test under the owning area exercises or imports `DSH_HOME`.
- [`apps/cli/tests/lazy-search-startup.compat.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/cli/tests/lazy-search-startup.compat.spec.ts) — A test under the owning area exercises or imports `DSH_HOME`.
- Source verification intent: Focused unit coverage pins the supplied SVG and Chinese copy hashes, version bumps, exclusive concurrent acknowledgement, malformed markers, persistence retry, Escape behavior, ASCII fallback, width-tier selection, bounded rendering, and low-height scrolling. Real Loader/PTY cases cover 60, 80, 120, and 160 columns plus a low-height viewport, emit semantic terminal snapshots, prove first launch then second-launch suppression under one DSH_HOME, and prove a resumed session appends no notice-derived user message or turn; ordinary terminal-exit lifecycle events remain unchanged.

## How to read the implementation

1. Start with [`apps/cli`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/apps/cli) because it has the strongest evidence link to the note.
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
- **`rust/trait-seam`** — Use a small trait at the capability boundary; keep policy and model-facing schemas outside the backend implementation.
- **Status gate** — Historical design. Use it to understand an older pressure or failure; locate the replacing decision before implementing anything.

### Smallest safe implementation slice

1. Express the semantic contract as Rust types and one owning boundary.
2. Add the smallest adapter or service needed by the existing loop; do not add a new model-facing tool unless the four-tool surface cannot express the job.
3. Record durable evidence before deriving summaries or UI state.
4. Add one happy-path test, one interruption test, and one replay/restart test where state survives a turn.
5. Keep the patch reversible until the source note's strongest failure case is proven in the Rust harness.

## Search handles

- Tags: `class/feature`, `concern/boundary`, `concern/compatibility`, `concern/concurrency`, `concern/discovery-routing`, `concern/evidence`, `concern/lifecycle`, `concern/ownership`, `concern/recovery`, `domain/build-release`, `domain/configuration`, `domain/extensions`, `domain/llm`, `domain/session-state`, `domain/shell-terminal`, `domain/storage`, `domain/testing`, `domain/ui-interaction`, `lifecycle/archived`, `mechanism/event-sourcing`, `mechanism/policy`
- Aliases: `dsh`, `DSH_HOME`, `ctx.tui.openOverlay`, `fallback. ANSI styling stays outside both the SVG and editable copy:`, `Versioned TUI first-run welcome`, `feature`, `boundary`, `compatibility`, `concurrency`, `discovery routing`, `evidence`, `lifecycle`, `ownership`, `recovery`
- Regex: `(?i)(DSH_HOME|ctx\.tui\.openOverlay|Versioned[- ]TUI[- ]first\-run[- ]welcome|feature|boundary|compatibility|concurrency|discovery[- ]routing)`

```bash
rg -n --pcre2 "(?i)(DSH_HOME|ctx\\.tui\\.openOverlay|Versioned[- ]TUI[- ]first\\-run[- ]welcome|feature|boundary|compatibility|concurrency|discovery[- ]routing)" source-deepseek-harness
rg -l --fixed-strings "class/feature" 03-agent-context
```

## Connected notes

- **`shares-code-with`** — [0676. Explicit-config dsh entrypoint](0676-explicit-config-dsh-entrypoint.md): Shares source implementation: `apps/cli`, `apps/cli/README.md`.
- **`shares-code-with`** — [0579. Product-level TUI session resume](0579-product-level-tui-session-resume.md): Shares source implementation: `apps/cli`, `apps/cli/README.md`.
- **`shares-code-with`** — [0647. the installer adopts an existing checkout into the managed layout](0647-the-installer-adopts-an-existing-checkout-into-the-managed-layout.md): Shares source implementation: `apps/cli`, `apps/cli/README.md`.
- **`shares-code-with`** — [0527. Artifact-first NPM baseline publication](0527-artifact-first-npm-baseline-publication.md): Shares source implementation: `apps/cli`, `apps/cli/README.md`.
- **`shares-code-with`** — [0402. Product-first root README](0402-product-first-root-readme.md): Shares source implementation: `apps/cli`, `apps/cli/README.md`.
- **`shares-code-with`** — [0597. `dsh meta` boots the TUI over the harness checkout](0597-dsh-meta-boots-the-tui-over-the-harness-checkout.md): Shares source implementation: `apps/cli`, `apps/cli/README.md`.
- **`shares-code-with`** — [0456. Remove the stdio and Echo agents](0456-remove-the-stdio-and-echo-agents.md): Shares source implementation: `apps/cli/README.md`, `apps/cli/tests/built-bin.e2e.ts`.
- **`shares-code-with`** — [0485. Source run without a managed installer](0485-source-run-without-a-managed-installer.md): Shares source implementation: `apps/cli`, `apps/cli/README.md`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0604-versioned-tui-first-run-welcome.md`.
