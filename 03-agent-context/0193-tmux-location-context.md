---
id: "dsh-note-0193"
title: "tmux-location context"
status: "implemented"
class: "feature"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/feature/2026-07-27-tmux-location-context.md"
implementation_evidence: "medium"
target_anchor: "sandbox capability boundary"
tags:
  - "class/feature"
  - "concern/boundary"
  - "concern/discovery-routing"
  - "concern/evidence"
  - "concern/ownership"
  - "concern/performance"
  - "concern/recovery"
  - "concern/trust"
  - "domain/agent-loop"
  - "domain/build-release"
  - "domain/configuration"
  - "domain/context"
  - "domain/extensions"
  - "domain/jobs-tasks"
  - "domain/llm"
  - "domain/security"
  - "domain/session-state"
  - "domain/shell-terminal"
  - "domain/storage"
  - "domain/testing"
  - "domain/ui-interaction"
  - "lifecycle/implemented"
  - "mechanism/event-sourcing"
  - "mechanism/policy"
  - "mechanism/projection"
aliases:
  - "shell"
  - "apply"
  - "refreshIntervalMs"
  - "resolve"
  - "run"
  - "$TMUX_PANE"
  - "tmux display-message -t \"$TMUX_PANE\" -p '<format>"
  - "@deepseek-ai/dsh-tmux-context"
  - "packages/context/tmux-context/"
  - "dsh-agent-spine-demo"
  - "agent/pre-step"
  - "ctx.shell"
  - "child_process"
  - "agent-instructions"
search_regex: "(?i)(shell|apply|refreshIntervalMs|resolve|\\$TMUX_PANE|tmux[- ]display\\-message[- ]\\-t[- ]\"\\$TMUX_PANE\"[- ]\\-p[- ]'<format>|@deepseek\\-ai/dsh\\-tmux\\-context|packages/context/tmux\\-context/)"
---

# 0193. tmux-location context — implementation context

## Open this when

An agent running inside tmux has no way to tell the model where it is: which session, window, and pane the process occupies, and how the window is laid out. A user directing several panes wants the model to orient itself to its own location so instructions like "the pane below" or "this window" resolve. The location must reach the model as durable, reconstructable context, not a system-prompt value rewritten in place, and must cost nothing when the location has not changed.

## Source decision

@deepseek-ai/dsh-tmux-context is an opt-in function plugin in packages/context/tmux-context/, alongside the other bounded request-context enrichments that define neither a tool nor a service. The shipped TUI mounts it because terminal-multiplexer context is specific to that surface; dsh-agent-spine-demo and the Web/headless surfaces stay silent. Pull on the first step of each turn, not a tmux push. The plugin prepends an agent/pre-step listener and acts only when step === 1.

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/feature/2026-07-27-tmux-location-context.md](../02-notes/implemented/feature/2026-07-27-tmux-location-context.md)
- Pinned source: [.agents/notes/implemented/feature/2026-07-27-tmux-location-context.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/feature/2026-07-27-tmux-location-context.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`packages/shell/shell/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/shell/shell/src/index.ts) | package entry point | Core file in the package named by the note: `packages/shell/shell`. | `named-package-member` |
| [`packages/shell/shell/src/types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/shell/shell/src/types.ts) | public types and contract | Core file in the package named by the note: `packages/shell/shell`. | `named-package-member` |
| [`packages/shell/shell/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/shell/shell/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/shell/shell`. Defines `apply`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/context/tmux-context/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/context/tmux-context/src/index.ts) | package entry point | Entry point or contract under the directory named by the note: `packages/context/tmux-context`. Core file in the package named by the note: `packages/context/tmux-context`. | `named-directory-member, named-package-member, symbol-definition` |
| [`packages/context/tmux-context/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/context/tmux-context/src/invariant.ts) | runtime contract checks | Entry point or contract under the directory named by the note: `packages/context/tmux-context`. Core file in the package named by the note: `packages/context/tmux-context`. | `named-directory-member, named-package-member, symbol-definition` |
| [`packages/examples/agent-spine-demo/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/examples/agent-spine-demo/src/index.ts) | package entry point | Core file in the package named by the note: `packages/examples/agent-spine-demo`. Defines `apply`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/context/agent-instructions/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/context/agent-instructions/src/index.ts) | package entry point | Core file in the package named by the note: `packages/context/agent-instructions`. Defines `apply`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/examples/agent-spine-demo/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/examples/agent-spine-demo/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/examples/agent-spine-demo`. | `named-package-member` |
| [`packages/context/agent-instructions/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/context/agent-instructions/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/context/agent-instructions`. | `named-package-member` |
| [`packages/context/tmux-context/README.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/context/tmux-context/README.md) | package contract and examples | Entry point or contract under the directory named by the note: `packages/context/tmux-context`. Core file in the package named by the note: `packages/context/tmux-context`. | `named-directory-member, named-package-member` |
| [`packages/context/tmux-context/package.json`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/context/tmux-context/package.json) | composition and configuration | Entry point or contract under the directory named by the note: `packages/context/tmux-context`. Core file in the package named by the note: `packages/context/tmux-context`. | `named-directory-member, named-package-member` |
| [`packages/context/tmux-context`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/packages/context/tmux-context) | package or module directory | The source note names this implementation area directly. The note names this package or capability. | `named-directory, named-package` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `shell` | `const` | [`packages/client/web/src/boot.tsx:121`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/web/src/boot.tsx#L121) | `const shell = this.ctx.get('appShell')` |
| `apply` | `function` | [`packages/context/agent-instructions/src/index.ts:80`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/context/agent-instructions/src/index.ts#L80) | `export function apply(ctx: Context, config: Config): void {` |
| `apply` | `function` | [`packages/context/tmux-context/src/index.ts:214`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/context/tmux-context/src/index.ts#L214) | `export function apply(ctx: Context, config: Config): void {` |
| `refreshIntervalMs` | `const` | [`packages/context/tmux-context/src/index.ts:215`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/context/tmux-context/src/index.ts#L215) | `const refreshIntervalMs = config.refreshIntervalMs` |
| `apply` | `const` | [`packages/context/tmux-context/src/invariant.ts:28`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/context/tmux-context/src/invariant.ts#L28) | `export const apply = (ctx: Context): Promise<() => void> =>` |
| `apply` | `function` | [`packages/examples/agent-spine-demo/src/index.ts:212`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/examples/agent-spine-demo/src/index.ts#L212) | `export function apply(ctx: Context, config: Config): void {` |
| `apply` | `const` | [`packages/shell/shell/src/invariant.ts:21`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/shell/shell/src/invariant.ts#L21) | `export const apply = (ctx: Context): Promise<() => void> =>` |
| `resolve` | `function` | [`vendor/cordis/src/registry.ts:71`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/cordis/src/registry.ts#L71) | `export function resolve(inject: Inject \| null \| undefined, result: Dict = Object.create(null)) {` |
| `run` | `const` | [`vendor/include/src/index.ts:226`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/include/src/index.ts#L226) | `const run = this.applyQueue.then(task, task)` |

### Tests and executable evidence

- [`packages/context/tmux-context/tests/tmux-context.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/context/tmux-context/tests/tmux-context.spec.ts) — A test under the owning area exercises or imports `$TMUX_PANE`. A test under the owning area exercises or imports `refreshIntervalMs`.
- [`packages/examples/agent-spine-demo/tests/agent-core.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/examples/agent-spine-demo/tests/agent-core.spec.ts) — A test under the owning area exercises or imports `dsh-agent-spine-demo`. A test under the owning area exercises or imports `agent-instructions`.
- [`packages/context/agent-instructions/tests/agent-instructions.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/context/agent-instructions/tests/agent-instructions.e2e.ts) — A test under the owning area exercises or imports `agent-instructions`.
- [`packages/context/agent-instructions/tests/agent-instructions.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/context/agent-instructions/tests/agent-instructions.spec.ts) — A test under the owning area exercises or imports `agent-instructions`.
- [`apps/web/tests/live-interactions.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/live-interactions.e2e.ts) — Contains the exact code literal `request/header` named by the note.
- [`apps/cli/tests/fixtures/dsh-badge/snapshot.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/cli/tests/fixtures/dsh-badge/snapshot.ts) — Contains the exact code literal `agent/pre-step` named by the note.
- Source verification intent: Unit tests pin: first-step injection and source/surface metadata; the $TMUX_PANE-keyed command including its #{pane_tty}-vs-ps -o tty= guard; step-gating; change suppression across turns and re-injection on a moved pane; positive-interval suppression and threshold; every no-op path (no bash, nonzero exit, wrong field count, empty pane id, aborted signal, and a contained executor rejection from either resolve() or run() that warns instead of failing the turn); prepended ordering before ordinary agent/pre-step listeners; resilience to a corrupt prior reading (non-text block, single-line text); and config.

## How to read the implementation

1. Start with [`packages/shell/shell/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/shell/shell/src/index.ts) because it has the strongest evidence link to the note.
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
- **`rust/context-evidence`** — Compaction may shrink presentation, but it must not erase the evidence ledger, file activity, or durable session facts.
- **`rust/four-tool-surface`** — Preserve the four model tools: read, write, patch, and exec. Reuse internal capabilities without automatically exposing new model-facing tools.
- **`rust/process-discipline`** — Treat this as repository or release policy unless it changes a runtime contract. Do not add runtime machinery for a process-only lesson.
- **`rust/provider-adapter`** — Keep provider wire quirks inside adapters; the loop and durable event vocabulary remain provider-neutral.
- **`rust/quickjs-boundary`** — QuickJS holds orchestration variables; Rust/Tokio owns async execution and exposes only explicitly granted capabilities.
- **`rust/tokio-cancellation`** — Thread one cancellation token through async work, close owned resources before reuse, and test cancellation at every await boundary.
- **`rust/trait-seam`** — Use a small trait at the capability boundary; keep policy and model-facing schemas outside the backend implementation.

### Smallest safe implementation slice

1. Express the semantic contract as Rust types and one owning boundary.
2. Add the smallest adapter or service needed by the existing loop; do not add a new model-facing tool unless the four-tool surface cannot express the job.
3. Record durable evidence before deriving summaries or UI state.
4. Add one happy-path test, one interruption test, and one replay/restart test where state survives a turn.
5. Keep the patch reversible until the source note's strongest failure case is proven in the Rust harness.

## Search handles

- Tags: `class/feature`, `concern/boundary`, `concern/discovery-routing`, `concern/evidence`, `concern/ownership`, `concern/performance`, `concern/recovery`, `concern/trust`, `domain/agent-loop`, `domain/build-release`, `domain/configuration`, `domain/context`, `domain/extensions`, `domain/jobs-tasks`, `domain/llm`, `domain/security`, `domain/session-state`, `domain/shell-terminal`, `domain/storage`, `domain/testing`, `domain/ui-interaction`, `lifecycle/implemented`, `mechanism/event-sourcing`, `mechanism/policy`, `mechanism/projection`
- Aliases: `shell`, `apply`, `refreshIntervalMs`, `resolve`, `run`, `$TMUX_PANE`, `tmux display-message -t "$TMUX_PANE" -p '<format>`, `@deepseek-ai/dsh-tmux-context`, `packages/context/tmux-context/`, `dsh-agent-spine-demo`, `agent/pre-step`, `ctx.shell`, `child_process`, `agent-instructions`
- Regex: `(?i)(shell|apply|refreshIntervalMs|resolve|\$TMUX_PANE|tmux[- ]display\-message[- ]\-t[- ]"\$TMUX_PANE"[- ]\-p[- ]'<format>|@deepseek\-ai/dsh\-tmux\-context|packages/context/tmux\-context/)`

```bash
rg -n --pcre2 "(?i)(shell|apply|refreshIntervalMs|resolve|\\$TMUX_PANE|tmux[- ]display\\-message[- ]\\-t[- ]\"\\$TMUX_PANE\"[- ]\\-p[- ]'<format>|@deepseek\\-ai/dsh\\-tmux\\-context|packages/context/tmux\\-context/)" source-deepseek-harness
rg -l --fixed-strings "class/feature" 03-agent-context
```

## Connected notes

- **`shares-code-with`** — [0287. Plugin configuration in the web settings page](0287-plugin-configuration-in-the-web-settings-page.md): Shares source implementation: `packages/shell/shell/src/index.ts`, `packages/shell/shell/src/invariant.ts`.
- **`shares-code-with`** — [0146. The subprocess sandbox --- confinement seam, native runners, escalation, and per-session modes](0146-the-subprocess-sandbox-confinement-seam-native-runners-escalation-and-pe.md): Shares source implementation: `packages/shell/shell/src/index.ts`, `packages/shell/shell/src/types.ts`.
- **`shares-code-with`** — [0009. Session persistence as an abstract service over the existing `SessionEvent`](0009-session-persistence-as-an-abstract-service-over-the-existing-sessioneven.md): Shares source implementation: `packages/shell/shell/src/index.ts`, `packages/shell/shell/src/invariant.ts`.
- **`shares-code-with`** — [0136. Workspace context instruction files](0136-workspace-context-instruction-files.md): Shares source implementation: `packages/context/agent-instructions/src/index.ts`, `packages/context/agent-instructions/src/invariant.ts`.
- **`shares-code-with`** — [0158. persistent PTY sessions](0158-persistent-pty-sessions.md): Shares source implementation: `packages/shell/shell/src/index.ts`, `packages/shell/shell/src/types.ts`.
- **`shares-code-with`** — [0066. The subprocess service is its own seam under the bash executors (`dsh-subprocess` / `dsh-subprocess-local`)](0066-the-subprocess-service-is-its-own-seam-under-the-bash-executors-dsh-subp.md): Shares source implementation: `packages/shell/shell/src/index.ts`, `packages/shell/shell/src/invariant.ts`.
- **`shares-code-with`** — [0011. Agent lifecycle and ownership contracts](0011-agent-lifecycle-and-ownership-contracts.md): Shares source implementation: `packages/shell/shell/src/index.ts`, `packages/shell/shell/src/invariant.ts`.
- **`shares-code-with`** — [0241. Windows defaults to pwsh](0241-windows-defaults-to-pwsh.md): Shares source implementation: `packages/shell/shell/src/index.ts`, `packages/shell/shell/src/invariant.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0193-tmux-location-context.md`.
