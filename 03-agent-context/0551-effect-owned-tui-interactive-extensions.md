---
id: "dsh-note-0551"
title: "Effect-owned TUI interactive extensions"
status: "archived"
class: "architecture"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/archived/architecture/2026-07-22-tui-interactive-extension-service.md"
implementation_evidence: "medium"
target_anchor: "sandbox capability boundary"
tags:
  - "class/architecture"
  - "concern/boundary"
  - "concern/cancellation-timeout"
  - "concern/concurrency"
  - "concern/discovery-routing"
  - "concern/evidence"
  - "concern/human-control"
  - "concern/lifecycle"
  - "concern/ownership"
  - "concern/recovery"
  - "concern/simplification"
  - "domain/configuration"
  - "domain/extensions"
  - "domain/llm"
  - "domain/security"
  - "domain/session-state"
  - "domain/shell-terminal"
  - "domain/storage"
  - "domain/testing"
  - "domain/ui-interaction"
  - "lifecycle/archived"
  - "mechanism/registry"
aliases:
  - "commands"
  - "ctx.commands"
  - "@deepseek-ai/dsh-tui"
  - "ctx.tui"
  - "ctx.tui.openOverlay"
  - "TUI"
  - "ctx.userInteraction"
  - "userInteraction"
  - "host.display"
  - "Effect-owned TUI interactive extensions"
  - "architecture"
  - "boundary"
  - "cancellation timeout"
  - "concurrency"
search_regex: "(?i)(commands|ctx\\.commands|@deepseek\\-ai/dsh\\-tui|ctx\\.tui|ctx\\.tui\\.openOverlay|ctx\\.userInteraction|userInteraction|host\\.display)"
---

# 0551. Effect-owned TUI interactive extensions — implementation context

## Open this when

Cordis plugins can register human commands through ctx.commands, but a command that needs terminal interaction has no supported presentation boundary. It must either remain non-interactive or capture the TUI's private pi-tui tree, focus state, renderer, and shutdown lifecycle. That coupling makes the extension depend on one front door's internals, lets independently developed overlays compete for focus, and leaves plugin unload with no reliable way to remove queued or visible UI.

## Source decision

A mounted @deepseek-ai/dsh-tui provides ctx.tui after terminal startup succeeds. The service belongs to that exact terminal and agent, disappears before terminal teardown, and causes plugins that inject it to unload and reload with provider availability. Other front doors do not emulate it. ctx.tui.openOverlay() is the first and only interactive extension primitive. It accepts a component factory, constrained layout options, and an optional abort signal. The factory receives a frozen host with the current viewport, semantic theme functions, display-text escaping, redraw, close, and a lifetime signal.

## Decision status

Historical design. Use it to understand an older pressure or failure; locate the replacing decision before implementing anything.

- Raw note: [archived/architecture/2026-07-22-tui-interactive-extension-service.md](../02-notes/archived/architecture/2026-07-22-tui-interactive-extension-service.md)
- Pinned source: [.agents/notes/archived/architecture/2026-07-22-tui-interactive-extension-service.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/archived/architecture/2026-07-22-tui-interactive-extension-service.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`packages/interaction/commands/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/interaction/commands/src/index.ts) | package entry point | Core file in the package named by the note: `packages/interaction/commands`. | `named-package-member` |
| [`packages/interaction/commands/src/types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/interaction/commands/src/types.ts) | public types and contract | Core file in the package named by the note: `packages/interaction/commands`. | `named-package-member` |
| [`packages/interaction/commands/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/interaction/commands/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/interaction/commands`. | `named-package-member` |
| [`packages/interaction/commands`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/packages/interaction/commands) | package or module directory | The note names this package or capability. | `named-package` |
| [`packages/hooks/hooks-codex/src/config.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/hooks/hooks-codex/src/config.ts) | runtime implementation | Defines `commands`, a construct named by the note. | `symbol-definition` |
| [`packages/interaction/commands/README.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/interaction/commands/README.md) | package contract and examples | Core file in the package named by the note: `packages/interaction/commands`. | `named-package-member` |
| [`packages/interaction/commands/package.json`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/interaction/commands/package.json) | composition and configuration | Core file in the package named by the note: `packages/interaction/commands`. | `named-package-member` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `commands` | `const` | [`packages/hooks/hooks-codex/src/config.ts:60`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/hooks/hooks-codex/src/config.ts#L60) | `const commands: MatcherGroup['hooks'] = []` |

### Tests and executable evidence

- No test file was tied to this note with enough confidence. Read the note's verification section and search the owning package's `tests/` directory.
- Source verification intent: Manager tests pin FIFO admission, cancellation, repeated close, shutdown outcomes, guarded callbacks, host capabilities, and per-file coverage. Cordis lifecycle tests pin caller ownership, provider loss and return, unloading-time rejection, and cleanup quiescence. Fake-terminal integration tests exercise plugin overlays alongside built-in questions, restored editor input, terminal remount, startup rollback, and service disappearance. Existing TUI interaction tests continue to exercise the model selector and question panel through the shared path.

## How to read the implementation

1. Start with [`packages/interaction/commands/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/interaction/commands/src/index.ts) because it has the strongest evidence link to the note.
2. Read public types and entry points before implementation details. Write down the contract in plain language.
3. Follow the named constructs into their callers and tests. Confirm which component can actually enforce the rule.
4. Reconstruct one failure path: cancellation, crash, retry, stale state, partial write, or incompatible provider input—whichever the note is protecting against.
5. Only then map the contract into the current Rust repository. Target paths must come from that repository, never from guesswork in this wiki.

## Rust harness port

**Likely integration point:** sandbox capability boundary.

Preserve the behavior and ownership rule. Do not mechanically translate Cordis, TypeScript declaration merging, Node streams, or package topology into Rust.

Relevant standing rules:

- **`rust/append-log`** — Keep durable facts append-only and derive views from them. Separate the synchronous fact boundary from asynchronous persistence.
- **`rust/capability-security`** — Make granted capabilities explicit. On the Windows no-admin path, probe support at runtime and fail closed rather than silently running unsandboxed.
- **`rust/four-tool-surface`** — Preserve the four model tools: read, write, patch, and exec. Reuse internal capabilities without automatically exposing new model-facing tools.
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

- Tags: `class/architecture`, `concern/boundary`, `concern/cancellation-timeout`, `concern/concurrency`, `concern/discovery-routing`, `concern/evidence`, `concern/human-control`, `concern/lifecycle`, `concern/ownership`, `concern/recovery`, `concern/simplification`, `domain/configuration`, `domain/extensions`, `domain/llm`, `domain/security`, `domain/session-state`, `domain/shell-terminal`, `domain/storage`, `domain/testing`, `domain/ui-interaction`, `lifecycle/archived`, `mechanism/registry`
- Aliases: `commands`, `ctx.commands`, `@deepseek-ai/dsh-tui`, `ctx.tui`, `ctx.tui.openOverlay`, `TUI`, `ctx.userInteraction`, `userInteraction`, `host.display`, `Effect-owned TUI interactive extensions`, `architecture`, `boundary`, `cancellation timeout`, `concurrency`
- Regex: `(?i)(commands|ctx\.commands|@deepseek\-ai/dsh\-tui|ctx\.tui|ctx\.tui\.openOverlay|ctx\.userInteraction|userInteraction|host\.display)`

```bash
rg -n --pcre2 "(?i)(commands|ctx\\.commands|@deepseek\\-ai/dsh\\-tui|ctx\\.tui|ctx\\.tui\\.openOverlay|ctx\\.userInteraction|userInteraction|host\\.display)" source-deepseek-harness
rg -l --fixed-strings "class/architecture" 03-agent-context
```

## Connected notes

- **`shares-code-with`** — [0163. Plugin-owned human command registration](0163-plugin-owned-human-command-registration.md): Shares source implementation: `packages/interaction/commands`, `packages/interaction/commands/README.md`.
- **`shares-code-with`** — [0202. `/feedback` command](0202-feedback-command.md): Shares source implementation: `packages/interaction/commands/README.md`, `packages/interaction/commands/package.json`.
- **`shares-code-with`** — [0215. Queued manual compaction with one durable lock](0215-queued-manual-compaction-with-one-durable-lock.md): Shares source implementation: `packages/interaction/commands`, `packages/interaction/commands/src/index.ts`.
- **`shares-code-with`** — [0475. Remove the TUI package](0475-remove-the-tui-package.md): Shares source implementation: `packages/interaction/commands`, `packages/interaction/commands/src/index.ts`.
- **`shares-code-with`** — [0160. Human `/goal` command](0160-human-goal-command.md): Shares source implementation: `packages/interaction/commands/src/index.ts`, `packages/interaction/commands/src/invariant.ts`.
- **`shares-code-with`** — [0062. Web command business surfaces and assembly (ui-commands / ui-skill / ui-subagent)](0062-web-command-business-surfaces-and-assembly-ui-commands-ui-skill-ui-subag.md): Shares source implementation: `packages/interaction/commands`, `packages/interaction/commands/src/index.ts`.
- **`shares-code-with`** — [0036. Shared scoped-layer storage](0036-shared-scoped-layer-storage.md): Shares source implementation: `packages/interaction/commands/src/index.ts`, `packages/interaction/commands/src/types.ts`.
- **`shares-code-with`** — [0114. An independent Events backstop closes the cordis-surface exhaustiveness gap](0114-an-independent-events-backstop-closes-the-cordis-surface-exhaustiveness.md): Shares source implementation: `packages/interaction/commands/src/index.ts`, `packages/interaction/commands/src/invariant.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0551-effect-owned-tui-interactive-extensions.md`.
