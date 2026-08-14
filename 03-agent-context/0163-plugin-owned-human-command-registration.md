---
id: "dsh-note-0163"
title: "Plugin-owned human command registration"
status: "implemented"
class: "feature"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/feature/2026-07-19-plugin-command-registration.md"
implementation_evidence: "medium"
target_anchor: "Rust provider-adapter boundary"
tags:
  - "class/feature"
  - "concern/boundary"
  - "concern/cancellation-timeout"
  - "concern/discovery-routing"
  - "concern/human-control"
  - "concern/lifecycle"
  - "concern/ownership"
  - "concern/performance"
  - "concern/schema-types"
  - "domain/build-release"
  - "domain/configuration"
  - "domain/extensions"
  - "domain/jobs-tasks"
  - "domain/llm"
  - "domain/observability"
  - "domain/protocols"
  - "domain/session-state"
  - "domain/shell-terminal"
  - "domain/storage"
  - "domain/testing"
  - "domain/ui-interaction"
  - "lifecycle/implemented"
  - "mechanism/adapter"
  - "mechanism/registry"
aliases:
  - "ctx"
  - "success"
  - "list"
  - "commands"
  - "goals"
  - "CommandDefinition"
  - "parseCommand"
  - "find"
  - "@deepseek-ai/dsh-commands"
  - "packages/interaction/commands/"
  - "at byte zero, a lowercase ASCII name containing letters, digits,"
  - "agent.ctx"
  - "commands/change"
  - "user/message"
search_regex: "(?i)(success|list|commands|goals|CommandDefinition|parseCommand|find|@deepseek\\-ai/dsh\\-commands)"
---

# 0163. Plugin-owned human command registration — implementation context

## Open this when

The TUI owns slash commands. Keeping command names, help text, autocomplete, dispatch, and cancellation inside the adapter makes every new command a TUI edit and prevents optional plugins from contributing commands. Treating slash input as an ordinary model prompt is also unsafe: a user-visible direct action can unexpectedly consume tokens or let the model reinterpret an unknown command. A shared mechanism must remain a UI concern rather than a model tool or agent-loop branch.

## Source decision

@deepseek-ai/dsh-commands in packages/interaction/commands/ is the product command registry. The TUI app bundle mounts it beside its consuming front end; the automation-only ACP app and the executor-less, UI-less agent spine omit it. TUI injects the service, while command producers depend only on the registry and any domain they operate. A CommandDefinition contains a lowercase name without /, a non-empty description, an optional unstructured-input hint, and an abortable handler. Registration validates and detaches the metadata, freezes the effective definition, and returns the exact Cordis effect disposer.

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/feature/2026-07-19-plugin-command-registration.md](../02-notes/implemented/feature/2026-07-19-plugin-command-registration.md)
- Pinned source: [.agents/notes/implemented/feature/2026-07-19-plugin-command-registration.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/feature/2026-07-19-plugin-command-registration.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`packages/core/agent-loop/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent-loop/src/index.ts) | package entry point | Core file in the package named by the note: `packages/core/agent-loop`. | `named-package-member` |
| [`packages/core/agent-loop/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent-loop/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/core/agent-loop`. | `named-package-member` |
| [`packages/interaction/commands/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/interaction/commands/src/index.ts) | package entry point | Entry point or contract under the directory named by the note: `packages/interaction/commands`. Core file in the package named by the note: `packages/interaction/commands`. | `named-directory-member, named-package-member, symbol-definition` |
| [`packages/interaction/commands/src/types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/interaction/commands/src/types.ts) | public types and contract | Entry point or contract under the directory named by the note: `packages/interaction/commands`. Core file in the package named by the note: `packages/interaction/commands`. | `named-directory-member, named-package-member` |
| [`packages/interaction/commands/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/interaction/commands/src/invariant.ts) | runtime contract checks | Entry point or contract under the directory named by the note: `packages/interaction/commands`. Core file in the package named by the note: `packages/interaction/commands`. | `named-directory-member, named-package-member` |
| [`packages/interaction/commands/README.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/interaction/commands/README.md) | package contract and examples | Entry point or contract under the directory named by the note: `packages/interaction/commands`. Core file in the package named by the note: `packages/interaction/commands`. | `named-directory-member, named-package-member` |
| [`packages/interaction/commands/package.json`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/interaction/commands/package.json) | composition and configuration | Entry point or contract under the directory named by the note: `packages/interaction/commands`. Core file in the package named by the note: `packages/interaction/commands`. | `named-directory-member, named-package-member` |
| [`packages/interaction/commands`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/packages/interaction/commands) | package or module directory | The source note names this implementation area directly. The note names this package or capability. | `named-directory, named-package` |
| [`packages/core/agent-loop`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent-loop) | package or module directory | The note names this package or capability. | `named-package` |
| [`scripts/rescope-vendor.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/rescope-vendor.ts) | repository automation | Defines `find`, a construct named by the note. | `symbol-definition` |
| [`apps/cli/src/profile-boot.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/cli/src/profile-boot.ts) | runtime implementation | Defines `ctx`, a construct named by the note. | `symbol-definition` |
| [`packages/host/apiproxy/src/api-proxy.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/src/api-proxy.ts) | runtime implementation | Defines `goals`, a construct named by the note. | `symbol-definition` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `ctx` | `const` | [`apps/cli/src/profile-boot.ts:248`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/cli/src/profile-boot.ts#L248) | `const ctx = await boot(NAME, rootConfig, structuredClone(allPatches(composed)), (hostCtx) => {` |
| `success` | `function` | [`packages/feedback/message-feedback/src/index.ts:91`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/feedback/message-feedback/src/index.ts#L91) | `function success<T>(value: T): MessageFeedbackSuccess<T> {` |
| `list` | `const` | [`packages/hooks/hook-protocol/src/merge.ts:75`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/hooks/hook-protocol/src/merge.ts#L75) | `const list = reasonsByRank.get(r) ?? []` |
| `commands` | `const` | [`packages/hooks/hooks-codex/src/config.ts:60`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/hooks/hooks-codex/src/config.ts#L60) | `const commands: MatcherGroup['hooks'] = []` |
| `goals` | `const` | [`packages/host/apiproxy/src/api-proxy.ts:1798`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/src/api-proxy.ts#L1798) | `const goals = presets?.serviceFor(agent, 'goals') ?? ctx.get('goals')` |
| `CommandDefinition` | `interface` | [`packages/interaction/commands/src/index.ts:40`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/interaction/commands/src/index.ts#L40) | `export interface CommandDefinition {` |
| `parseCommand` | `function` | [`packages/interaction/commands/src/index.ts:102`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/interaction/commands/src/index.ts#L102) | `export function parseCommand(line: string): ParsedCommand \| undefined {` |
| `find` | `const` | [`scripts/rescope-vendor.ts:615`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/rescope-vendor.ts#L615) | `const find = reverse ? edit.replace : edit.find` |

### Tests and executable evidence

- [`packages/core/agent-loop/tests/loop.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent-loop/tests/loop.spec.ts) — A test under the owning area exercises or imports `dsh-agent-loop`.
- [`packages/core/agent-loop/tests/resume.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent-loop/tests/resume.spec.ts) — A test under the owning area exercises or imports `AbortController`.
- [`packages/core/agent-loop/tests/settings.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent-loop/tests/settings.spec.ts) — A test under the owning area exercises or imports `dsh-agent-loop`.
- [`packages/core/agent-loop/tests/tool-calls.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent-loop/tests/tool-calls.spec.ts) — A test under the owning area exercises or imports `dsh-agent-loop`.
- [`packages/core/agent-loop/tests/properties.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent-loop/tests/properties.spec.ts) — A test under the owning area exercises or imports `dsh-agent-loop`.
- [`packages/interaction/commands/tests/commands.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/interaction/commands/tests/commands.spec.ts) — A test under the owning area exercises or imports `CommandDefinition`. A test under the owning area exercises or imports `parseCommand`.
- [`packages/core/agent-loop/tests/scope-lifecycle.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent-loop/tests/scope-lifecycle.spec.ts) — A test under the owning area exercises or imports `AbortController`. A test under the owning area exercises or imports `dsh-agent-loop`.
- [`packages/core/agent-loop/tests/agent-initiator.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent-loop/tests/agent-initiator.spec.ts) — A test under the owning area exercises or imports `AbortController`.
- Source verification intent: The registry suite covers syntax boundaries, immutable normalization, runtime metadata validation, deterministic sorting, global and scoped shadowing, duplicate rejection, exact disposal, contained change-notification failures, direct invocation, expected and malformed results, synchronous and asynchronous failure, and every abort timing edge at per-file 100% statement, branch, function, and line coverage.

## How to read the implementation

1. Start with [`packages/core/agent-loop/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent-loop/src/index.ts) because it has the strongest evidence link to the note.
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
- **`rust/newtype-ids`** — Represent identity domains with Rust newtypes; do not pass bare strings across session, call, turn, step, and agent boundaries.
- **`rust/process-discipline`** — Treat this as repository or release policy unless it changes a runtime contract. Do not add runtime machinery for a process-only lesson.
- **`rust/provider-adapter`** — Keep provider wire quirks inside adapters; the loop and durable event vocabulary remain provider-neutral.
- **`rust/quickjs-boundary`** — QuickJS holds orchestration variables; Rust/Tokio owns async execution and exposes only explicitly granted capabilities.
- **`rust/serde-validation`** — Parse untrusted input into strict serde types and validate semantic constraints before executing side effects.
- **`rust/tagged-enums`** — Use serde-tagged enums for a closed wire vocabulary. Add an explicit extension envelope only where runtime extension is truly required.
- **`rust/tokio-cancellation`** — Thread one cancellation token through async work, close owned resources before reuse, and test cancellation at every await boundary.
- **`rust/trait-seam`** — Use a small trait at the capability boundary; keep policy and model-facing schemas outside the backend implementation.

### Smallest safe implementation slice

1. Express the semantic contract as Rust types and one owning boundary.
2. Add the smallest adapter or service needed by the existing loop; do not add a new model-facing tool unless the four-tool surface cannot express the job.
3. Record durable evidence before deriving summaries or UI state.
4. Add one happy-path test, one interruption test, and one replay/restart test where state survives a turn.
5. Keep the patch reversible until the source note's strongest failure case is proven in the Rust harness.

## Search handles

- Tags: `class/feature`, `concern/boundary`, `concern/cancellation-timeout`, `concern/discovery-routing`, `concern/human-control`, `concern/lifecycle`, `concern/ownership`, `concern/performance`, `concern/schema-types`, `domain/build-release`, `domain/configuration`, `domain/extensions`, `domain/jobs-tasks`, `domain/llm`, `domain/observability`, `domain/protocols`, `domain/session-state`, `domain/shell-terminal`, `domain/storage`, `domain/testing`, `domain/ui-interaction`, `lifecycle/implemented`, `mechanism/adapter`, `mechanism/registry`
- Aliases: `ctx`, `success`, `list`, `commands`, `goals`, `CommandDefinition`, `parseCommand`, `find`, `@deepseek-ai/dsh-commands`, `packages/interaction/commands/`, `at byte zero, a lowercase ASCII name containing letters, digits,`, `agent.ctx`, `commands/change`, `user/message`
- Regex: `(?i)(success|list|commands|goals|CommandDefinition|parseCommand|find|@deepseek\-ai/dsh\-commands)`

```bash
rg -n --pcre2 "(?i)(success|list|commands|goals|CommandDefinition|parseCommand|find|@deepseek\\-ai/dsh\\-commands)" source-deepseek-harness
rg -l --fixed-strings "class/feature" 03-agent-context
```

## Connected notes

- **`source-link`** — [0459. ACP as an automation-only protocol](0459-acp-as-an-automation-only-protocol.md): The source note links to this decision directly.
- **`shares-code-with`** — [0551. Effect-owned TUI interactive extensions](0551-effect-owned-tui-interactive-extensions.md): Shares source implementation: `packages/interaction/commands`, `packages/interaction/commands/README.md`.
- **`shares-code-with`** — [0202. `/feedback` command](0202-feedback-command.md): Shares source implementation: `packages/interaction/commands/README.md`, `packages/interaction/commands/package.json`.
- **`shares-code-with`** — [0215. Queued manual compaction with one durable lock](0215-queued-manual-compaction-with-one-durable-lock.md): Shares source implementation: `packages/interaction/commands`, `packages/interaction/commands/src/index.ts`.
- **`shares-code-with`** — [0160. Human `/goal` command](0160-human-goal-command.md): Shares source implementation: `packages/interaction/commands/src/index.ts`, `packages/interaction/commands/src/invariant.ts`.
- **`shares-code-with`** — [0062. Web command business surfaces and assembly (ui-commands / ui-skill / ui-subagent)](0062-web-command-business-surfaces-and-assembly-ui-commands-ui-skill-ui-subag.md): Shares source implementation: `packages/interaction/commands`, `packages/interaction/commands/src/index.ts`.
- **`shares-code-with`** — [0475. Remove the TUI package](0475-remove-the-tui-package.md): Shares source implementation: `packages/interaction/commands`, `packages/interaction/commands/src/index.ts`.
- **`shares-code-with`** — [0305. Semantic session checkpoints](0305-semantic-session-checkpoints.md): Shares source implementation: `packages/core/agent-loop`, `packages/core/agent-loop/src/index.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0163-plugin-owned-human-command-registration.md`.
