---
id: "dsh-note-0596"
title: "`dsh migrate`/`dsh upgrade` seed the first turn with a skill"
status: "archived"
class: "feature"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/archived/feature/2026-07-28-dsh-guided-skill-session-commands.md"
implementation_evidence: "high"
target_anchor: "read/write/patch filesystem capability"
tags:
  - "class/feature"
  - "concern/boundary"
  - "concern/discovery-routing"
  - "concern/evidence"
  - "concern/lifecycle"
  - "concern/recovery"
  - "concern/schema-types"
  - "domain/configuration"
  - "domain/extensions"
  - "domain/filesystem"
  - "domain/llm"
  - "domain/protocols"
  - "domain/session-state"
  - "domain/shell-terminal"
  - "domain/storage"
  - "domain/testing"
  - "domain/ui-interaction"
  - "lifecycle/archived"
  - "mechanism/adapter"
  - "mechanism/policy"
aliases:
  - "web"
  - "CONFIGURED_AGENT_IDENTITIES_KEY"
  - "Config"
  - "meta"
  - "apply"
  - "/skill:dsh-migrate"
  - "/skill:dsh-upgrade"
  - "dsh-migrate"
  - "dsh-upgrade"
  - "/skill:<name>"
  - "createTuiChat"
  - "invokeSkill"
  - "INITIAL_SKILL_KEY"
  - "tuiInitialSkill"
search_regex: "(?i)(CONFIGURED_AGENT_IDENTITIES_KEY|Config|meta|apply|/skill:dsh\\-migrate|/skill:dsh\\-upgrade|dsh\\-migrate|dsh\\-upgrade)"
---

# 0596. `dsh migrate`/`dsh upgrade` seed the first turn with a skill — implementation context

## Open this when

Two recurring flows begin with the user manually invoking one skill and answering its questions: migrating from another coding agent, and upgrading this checkout. Both require the user to know the skill exists and to type /skill:dsh-migrate or /skill:dsh-upgrade as the session's first turn. A dedicated entry command that drops the user straight into that guided session removes the discovery step.

## Source decision

dsh migrate and dsh upgrade boot the ordinary TUI as a fresh session whose first turn auto-invokes a bundled skill (dsh-migrate, dsh-upgrade), exactly as if the user typed /skill: and pressed Enter. The seed reuses the existing TUI skill path, not a new one. createTuiChat already has invokeSkill(name, instructions) --- the code a typed /skill: runs, including the "Unknown skill" notice.

## Decision status

Historical design. Use it to understand an older pressure or failure; locate the replacing decision before implementing anything.

- Raw note: [archived/feature/2026-07-28-dsh-guided-skill-session-commands.md](../02-notes/archived/feature/2026-07-28-dsh-guided-skill-session-commands.md)
- Pinned source: [.agents/notes/archived/feature/2026-07-28-dsh-guided-skill-session-commands.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/archived/feature/2026-07-28-dsh-guided-skill-session-commands.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`packages/core/agent-loop/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent-loop/src/index.ts) | package entry point | Core file in the package named by the note: `packages/core/agent-loop`. Defines `CONFIGURED_AGENT_IDENTITIES_KEY`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/hooks/hooks-codex/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/hooks/hooks-codex/src/index.ts) | package entry point | Core file in the package named by the note: `packages/hooks/hooks-codex`. Defines `apply`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/core/agent-loop/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent-loop/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/core/agent-loop`. Defines `apply`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/hooks/hooks-codex/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/hooks/hooks-codex/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/hooks/hooks-codex`. Defines `apply`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`apps/web`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/apps/web) | package or module directory | The note names this package or capability. | `named-package` |
| [`packages/core/agent-loop`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent-loop) | package or module directory | The note names this package or capability. | `named-package` |
| [`packages/hooks/hooks-codex`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/packages/hooks/hooks-codex) | package or module directory | The note names this package or capability. | `named-package` |
| [`apps/cli/src/args.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/cli/src/args.ts) | runtime implementation | Defines `web`, a construct named by the note. | `symbol-definition` |
| [`apps/web/package.json`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/package.json) | composition and configuration | Core file in the package named by the note: `apps/web`. | `named-package-member` |
| [`packages/core/agent-loop/README.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent-loop/README.md) | package contract and examples | Core file in the package named by the note: `packages/core/agent-loop`. | `named-package-member` |
| [`packages/hooks/hooks-codex/README.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/hooks/hooks-codex/README.md) | package contract and examples | Core file in the package named by the note: `packages/hooks/hooks-codex`. | `named-package-member` |
| [`packages/core/agent-loop/package.json`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent-loop/package.json) | composition and configuration | Core file in the package named by the note: `packages/core/agent-loop`. | `named-package-member` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `web` | `const` | [`apps/cli/src/args.ts:156`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/cli/src/args.ts#L156) | `const web = program.command('web').description('boot the web profile (alias of --profile web); the web app\'s own flags follow')` |
| `CONFIGURED_AGENT_IDENTITIES_KEY` | `const` | [`packages/core/agent-loop/src/index.ts:211`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent-loop/src/index.ts#L211) | `export const CONFIGURED_AGENT_IDENTITIES_KEY = 'configuredAgentIdentities'` |
| `Config` | `interface` | [`packages/core/agent-loop/src/index.ts:255`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent-loop/src/index.ts#L255) | `export interface Config {` |
| `meta` | `const` | [`packages/core/agent-loop/src/index.ts:356`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent-loop/src/index.ts#L356) | `const meta = cwd === undefined ? {} : { cwd }` |
| `apply` | `const` | [`packages/core/agent-loop/src/invariant.ts:62`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent-loop/src/invariant.ts#L62) | `export const apply = (ctx: Context): Promise<() => void> =>` |
| `Config` | `interface` | [`packages/hooks/hooks-codex/src/index.ts:44`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/hooks/hooks-codex/src/index.ts#L44) | `export interface Config {` |
| `Config` | `const` | [`packages/hooks/hooks-codex/src/index.ts:60`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/hooks/hooks-codex/src/index.ts#L60) | `export const Config: z<Config> = z.object({` |
| `apply` | `function` | [`packages/hooks/hooks-codex/src/index.ts:81`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/hooks/hooks-codex/src/index.ts#L81) | `export function apply(ctx: Context, config: Config): void {` |
| `apply` | `const` | [`packages/hooks/hooks-codex/src/invariant.ts:28`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/hooks/hooks-codex/src/invariant.ts#L28) | `export const apply = (ctx: Context): Promise<() => void> =>` |

### Tests and executable evidence

- [`apps/cli/tests/args.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/cli/tests/args.spec.ts) — The source note names this file directly.
- [`apps/web/tests/scaffold.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/scaffold.ts) — A test under the owning area exercises or imports `provide`. A test under the owning area exercises or imports `DSH_BUNDLED_SKILL_DIR`.
- [`apps/web/tests/scaffold-hermetic.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/scaffold-hermetic.e2e.ts) — A test under the owning area exercises or imports `DSH_BUNDLED_SKILL_DIR`.
- [`packages/hooks/hooks-codex/tests/bridge.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/hooks/hooks-codex/tests/bridge.spec.ts) — A test under the owning area exercises or imports `hooks-codex`.
- [`packages/hooks/hooks-codex/tests/coverage-cases.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/hooks/hooks-codex/tests/coverage-cases.ts) — A test under the owning area exercises or imports `hooks-codex`.
- [`packages/core/agent-loop/tests/config-session-id.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent-loop/tests/config-session-id.spec.ts) — A test under the owning area exercises or imports `CONFIGURED_AGENT_IDENTITIES_KEY`.
- [`apps/web/tests/snapshots/approval-composer/session.jsonl`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/snapshots/approval-composer/session.jsonl) — A test under the owning area exercises or imports `provide`.
- [`apps/web/tests/snapshots/permission-policy-context/session.jsonl`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/snapshots/permission-policy-context/session.jsonl) — A test under the owning area exercises or imports `provide`.
- Source verification intent: apps/cli/tests/args.spec.ts gains routing for migrate/upgrade (bare discriminant) and exit-1 for every leaked option on either side of each subcommand. packages/ui/tui/tests/tui.spec.ts gains two fake-terminal cases in the existing skill describe block: config.initialSkill set delivers the rendered skill body as the first turn with no user input, and an unknown initial skill reports a notice without sending. runSkillSession itself is composition inside the module's v8 ignore block, like runTui/runMeta.

## How to read the implementation

1. Start with [`packages/core/agent-loop/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent-loop/src/index.ts) because it has the strongest evidence link to the note.
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
- **`rust/newtype-ids`** — Represent identity domains with Rust newtypes; do not pass bare strings across session, call, turn, step, and agent boundaries.
- **`rust/provider-adapter`** — Keep provider wire quirks inside adapters; the loop and durable event vocabulary remain provider-neutral.
- **`rust/quickjs-boundary`** — QuickJS holds orchestration variables; Rust/Tokio owns async execution and exposes only explicitly granted capabilities.
- **`rust/serde-validation`** — Parse untrusted input into strict serde types and validate semantic constraints before executing side effects.
- **`rust/tagged-enums`** — Use serde-tagged enums for a closed wire vocabulary. Add an explicit extension envelope only where runtime extension is truly required.
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

- Tags: `class/feature`, `concern/boundary`, `concern/discovery-routing`, `concern/evidence`, `concern/lifecycle`, `concern/recovery`, `concern/schema-types`, `domain/configuration`, `domain/extensions`, `domain/filesystem`, `domain/llm`, `domain/protocols`, `domain/session-state`, `domain/shell-terminal`, `domain/storage`, `domain/testing`, `domain/ui-interaction`, `lifecycle/archived`, `mechanism/adapter`, `mechanism/policy`
- Aliases: `web`, `CONFIGURED_AGENT_IDENTITIES_KEY`, `Config`, `meta`, `apply`, `/skill:dsh-migrate`, `/skill:dsh-upgrade`, `dsh-migrate`, `dsh-upgrade`, `/skill:<name>`, `createTuiChat`, `invokeSkill`, `INITIAL_SKILL_KEY`, `tuiInitialSkill`
- Regex: `(?i)(CONFIGURED_AGENT_IDENTITIES_KEY|Config|meta|apply|/skill:dsh\-migrate|/skill:dsh\-upgrade|dsh\-migrate|dsh\-upgrade)`

```bash
rg -n --pcre2 "(?i)(CONFIGURED_AGENT_IDENTITIES_KEY|Config|meta|apply|/skill:dsh\\-migrate|/skill:dsh\\-upgrade|dsh\\-migrate|dsh\\-upgrade)" source-deepseek-harness
rg -l --fixed-strings "class/feature" 03-agent-context
```

## Connected notes

- **`source-link`** — [0580. TUI skill slash command](0580-tui-skill-slash-command.md): The source note links to this decision directly.
- **`source-link`** — [0607. experimental subcommands gate behind `--experimental` or `DSH_EXPERIMENTAL=1`](0607-experimental-subcommands-gate-behind-experimental-or-dsh-experimental-1.md): The source note links to this decision directly.
- **`shares-code-with`** — [0597. `dsh meta` boots the TUI over the harness checkout](0597-dsh-meta-boots-the-tui-over-the-harness-checkout.md): Shares source implementation: `apps/cli/src/args.ts`, `apps/cli/tests/args.spec.ts`.
- **`shares-code-with`** — [0679. Hook snapshot matrix --- end-to-end expected outputs for both bridges](0679-hook-snapshot-matrix-end-to-end-expected-outputs-for-both-bridges.md): Shares source implementation: `packages/hooks/hooks-codex`, `packages/hooks/hooks-codex/src/index.ts`.
- **`shares-code-with`** — [0163. Plugin-owned human command registration](0163-plugin-owned-human-command-registration.md): Shares source implementation: `packages/core/agent-loop`, `packages/core/agent-loop/src/index.ts`.
- **`shares-code-with`** — [0305. Semantic session checkpoints](0305-semantic-session-checkpoints.md): Shares source implementation: `packages/core/agent-loop`, `packages/core/agent-loop/src/index.ts`.
- **`shares-code-with`** — [0553. Parse `dsh` argv through one Commander adapter](0553-parse-dsh-argv-through-one-commander-adapter.md): Shares source implementation: `apps/cli/src/args.ts`, `apps/cli/tests/args.spec.ts`.
- **`shares-code-with`** — [0672. Replace tool-web's regex HTML-to-markdown converter with turndown](0672-replace-tool-web-s-regex-html-to-markdown-converter-with-turndown.md): Shares source implementation: `apps/cli/src/args.ts`, `apps/web`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0596-dsh-migrate-dsh-upgrade-seed-the-first-turn-with-a-skill.md`.
