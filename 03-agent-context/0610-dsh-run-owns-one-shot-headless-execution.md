---
id: "dsh-note-0610"
title: "`dsh run` owns one-shot headless execution"
status: "archived"
class: "feature"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/archived/feature/2026-08-08-dsh-run-headless-command.md"
implementation_evidence: "medium"
target_anchor: "append-only event and session store"
tags:
  - "class/feature"
  - "concern/boundary"
  - "concern/evidence"
  - "concern/lifecycle"
  - "concern/ownership"
  - "domain/build-release"
  - "domain/configuration"
  - "domain/extensions"
  - "domain/session-state"
  - "domain/shell-terminal"
  - "domain/storage"
  - "lifecycle/archived"
aliases:
  - "DshInvocation"
  - "runProfile"
  - "run"
  - "task"
  - "dsh --profile headless <task...>"
  - "--profile"
  - "--patch"
  - "RunInvocation"
  - "headless-runner"
  - "dsh run --profile <name> \"<task>"
  - "dsh -p"
  - "--profile headless"
  - "apps/cli/src/run.ts"
  - "`dsh run` owns one-shot headless execution"
search_regex: "(?i)(DshInvocation|runProfile|task|dsh[- ]\\-\\-profile[- ]headless[- ]<task\\.\\.\\.>|\\-\\-profile|\\-\\-patch|RunInvocation|headless\\-runner)"
---

# 0610. `dsh run` owns one-shot headless execution — implementation context

## Open this when

Generic profile boot and one-shot task execution have different lifecycle contracts. A root grammar that accepts optional task text makes one argv shape mean either a long-lived process or a terminating task according to a plugin row discovered only after composition. It also exposes a profile implementation detail as the primary user command and gives custom profiles no explicit one-shot entry. The run verb must have one top-level meaning. Sharing it with application-file execution or inferring its meaning from positional shape creates the same ambiguity.

## Source decision

One-shot execution owns this grammar: --profile defaults to headless and supports custom one-shot compositions. --patch is repeatable and occupies the normal overlay layer. Commander joins the variadic task arguments with spaces and rejects a missing or blank task before boot. RunInvocation is a distinct DshInvocation member. The generic profile invocation carries no task state and accepts no positional arguments. Both dispatch paths use runProfile: profile boot omits task, while run supplies it.

## Decision status

Historical design. Use it to understand an older pressure or failure; locate the replacing decision before implementing anything.

- Raw note: [archived/feature/2026-08-08-dsh-run-headless-command.md](../02-notes/archived/feature/2026-08-08-dsh-run-headless-command.md)
- Pinned source: [.agents/notes/archived/feature/2026-08-08-dsh-run-headless-command.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/archived/feature/2026-08-08-dsh-run-headless-command.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`packages/bundle/headless/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/bundle/headless/src/index.ts) | package entry point | Core file in the package named by the note: `packages/bundle/headless`. Defines `run`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/bundle/headless/src/startup.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/bundle/headless/src/startup.ts) | runtime implementation | Core file in the package named by the note: `packages/bundle/headless`. Defines `task`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/bundle/headless/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/bundle/headless/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/bundle/headless`. | `named-package-member` |
| [`packages/bundle/headless`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/packages/bundle/headless) | package or module directory | The note names this package or capability. | `named-package` |
| [`apps/cli/src/args.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/cli/src/args.ts) | runtime implementation | Defines `DshInvocation`, a construct named by the note. | `symbol-definition` |
| [`apps/cli/src/profile-boot.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/cli/src/profile-boot.ts) | runtime implementation | Defines `runProfile`, a construct named by the note. | `symbol-definition` |
| [`packages/bundle/headless/README.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/bundle/headless/README.md) | package contract and examples | Core file in the package named by the note: `packages/bundle/headless`. | `named-package-member` |
| [`packages/bundle/headless/package.json`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/bundle/headless/package.json) | composition and configuration | Core file in the package named by the note: `packages/bundle/headless`. | `named-package-member` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `DshInvocation` | `type` | [`apps/cli/src/args.ts:48`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/cli/src/args.ts#L48) | `export type DshInvocation = ProfileInvocation \| DumpConfigInvocation \| PluginInvocation` |
| `runProfile` | `function` | [`apps/cli/src/profile-boot.ts:207`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/cli/src/profile-boot.ts#L207) | `export async function runProfile(options: RunProfileOptions): Promise<{ ctx: Context; shutdown: ProcessShutdown }> {` |
| `run` | `function` | [`packages/bundle/headless/src/index.ts:96`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/bundle/headless/src/index.ts#L96) | `async function run(ctx: Context, task: string, io: HeadlessIo): Promise<void> {` |
| `task` | `const` | [`packages/bundle/headless/src/startup.ts:52`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/bundle/headless/src/startup.ts#L52) | `const task = program.args.join(' ')` |

### Tests and executable evidence

- [`packages/bundle/headless/tests/startup.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/bundle/headless/tests/startup.spec.ts) — A test under the owning area exercises or imports `headless-runner`.

## How to read the implementation

1. Start with [`packages/bundle/headless/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/bundle/headless/src/index.ts) because it has the strongest evidence link to the note.
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
- **`rust/process-discipline`** — Treat this as repository or release policy unless it changes a runtime contract. Do not add runtime machinery for a process-only lesson.
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

- Tags: `class/feature`, `concern/boundary`, `concern/evidence`, `concern/lifecycle`, `concern/ownership`, `domain/build-release`, `domain/configuration`, `domain/extensions`, `domain/session-state`, `domain/shell-terminal`, `domain/storage`, `lifecycle/archived`
- Aliases: `DshInvocation`, `runProfile`, `run`, `task`, `dsh --profile headless <task...>`, `--profile`, `--patch`, `RunInvocation`, `headless-runner`, `dsh run --profile <name> "<task>`, `dsh -p`, `--profile headless`, `apps/cli/src/run.ts`, ``dsh run` owns one-shot headless execution`
- Regex: `(?i)(DshInvocation|runProfile|task|dsh[- ]\-\-profile[- ]headless[- ]<task\.\.\.>|\-\-profile|\-\-patch|RunInvocation|headless\-runner)`

```bash
rg -n --pcre2 "(?i)(DshInvocation|runProfile|task|dsh[- ]\\-\\-profile[- ]headless[- ]<task\\.\\.\\.>|\\-\\-profile|\\-\\-patch|RunInvocation|headless\\-runner)" source-deepseek-harness
rg -l --fixed-strings "class/feature" 03-agent-context
```

## Connected notes

- **`shares-code-with`** — [0101. Profile plugin bundles replace the fixed surface overlays](0101-profile-plugin-bundles-replace-the-fixed-surface-overlays.md): Shares source implementation: `apps/cli/src/args.ts`, `apps/cli/src/profile-boot.ts`.
- **`shares-code-with`** — [0479. Remove the separate CLI demo](0479-remove-the-separate-cli-demo.md): Shares source implementation: `packages/bundle/headless`, `packages/bundle/headless/src/index.ts`.
- **`shares-code-with`** — [0115. headless is a direct core entry point](0115-headless-is-a-direct-core-entry-point.md): Shares source implementation: `packages/bundle/headless/src/index.ts`, `packages/bundle/headless/src/invariant.ts`.
- **`shares-code-with`** — [0456. Remove the stdio and Echo agents](0456-remove-the-stdio-and-echo-agents.md): Shares source implementation: `packages/bundle/headless/src/index.ts`, `packages/bundle/headless/src/invariant.ts`.
- **`shares-code-with`** — [0105. Apps own their command line through `ctx.cmdlineArgs`](0105-apps-own-their-command-line-through-ctx-cmdlineargs.md): Shares source implementation: `packages/bundle/headless/src/index.ts`, `packages/bundle/headless/src/startup.ts`.
- **`shares-code-with`** — [0597. `dsh meta` boots the TUI over the harness checkout](0597-dsh-meta-boots-the-tui-over-the-harness-checkout.md): Shares source implementation: `apps/cli/src/args.ts`.
- **`shares-code-with`** — [0338. Bounded, escalating signal shutdown for Web and headless](0338-bounded-escalating-signal-shutdown-for-web-and-headless.md): Shares source implementation: `apps/cli/src/profile-boot.ts`.
- **`shares-code-with`** — [0596. `dsh migrate`/`dsh upgrade` seed the first turn with a skill](0596-dsh-migrate-dsh-upgrade-seed-the-first-turn-with-a-skill.md): Shares source implementation: `apps/cli/src/args.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0610-dsh-run-owns-one-shot-headless-execution.md`.
