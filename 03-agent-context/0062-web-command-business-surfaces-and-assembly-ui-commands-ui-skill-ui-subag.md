---
id: "dsh-note-0062"
title: "Web command business surfaces and assembly (ui-commands / ui-skill / ui-subagent)"
status: "implemented"
class: "architecture"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/architecture/2026-07-25-web-command-surfaces-and-assembly.md"
implementation_evidence: "medium"
target_anchor: "read/write/patch filesystem capability"
tags:
  - "class/architecture"
  - "concern/boundary"
  - "concern/concurrency"
  - "concern/discovery-routing"
  - "concern/lifecycle"
  - "concern/ownership"
  - "concern/performance"
  - "concern/recovery"
  - "domain/context"
  - "domain/extensions"
  - "domain/filesystem"
  - "domain/llm"
  - "domain/multi-agent"
  - "domain/session-state"
  - "domain/shell-terminal"
  - "domain/storage"
  - "domain/testing"
  - "lifecycle/implemented"
  - "mechanism/generation"
  - "mechanism/policy"
  - "mechanism/projection"
  - "mechanism/registry"
aliases:
  - "CommandDirectory"
  - "PopupSelectController"
  - "CommandUiRuntime"
  - "lexicon"
  - "SessionId"
  - "input"
  - "list"
  - "commands"
  - "skills"
  - "available"
  - "ctx.commands"
  - "ctx.skills"
  - "ClientSessionContext { sessionId }"
  - "{sessionId}"
search_regex: "(?i)(CommandDirectory|PopupSelectController|CommandUiRuntime|lexicon|SessionId|input|list|commands)"
---

# 0062. Web command business surfaces and assembly (ui-commands / ui-skill / ui-subagent) — implementation context

## Open this when

The pipeline was ready but command knowledge had no landing spot: host-side ctx.commands and ctx.skills were complete while the web channel had no command capability. The business layer had to answer: Command UI takes more than one shape (execute on the spot, pop a select box, backfill and keep typing arguments) --- how do business packages ship with zero skeleton changes; When is the directory fetched: pulling on every menu open is too slow, while a resident cache needs invalidation and reconnect stories; Sessions are always agent-backed (Session + Agent born in the same instant) --- by what address does.

## Source decision

The ClientSessionContext { sessionId } projection is self-held in the ui-input-trigger contract (types.ts): sessions are always agent-backed, so session identity is the entire projection of command capability; the wire addresses by {sessionId} (both command.list and command.execute; the host resolves the Agent from the session header). The directory is compartmented by SessionId, with per-key single-flight + an epoch guard (an old pull never overwrites newer state); commands/changed soft-invalidates every key (the old snapshot keeps serving while the repull runs in the background), connection/reset.

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/architecture/2026-07-25-web-command-surfaces-and-assembly.md](../02-notes/implemented/architecture/2026-07-25-web-command-surfaces-and-assembly.md)
- Pinned source: [.agents/notes/implemented/architecture/2026-07-25-web-command-surfaces-and-assembly.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/architecture/2026-07-25-web-command-surfaces-and-assembly.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`packages/interaction/commands/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/interaction/commands/src/index.ts) | package entry point | Core file in the package named by the note: `packages/interaction/commands`. | `named-package-member` |
| [`packages/interaction/commands/src/types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/interaction/commands/src/types.ts) | public types and contract | Core file in the package named by the note: `packages/interaction/commands`. | `named-package-member` |
| [`packages/interaction/commands/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/interaction/commands/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/interaction/commands`. | `named-package-member` |
| [`apps/cli/README.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/cli/README.md) | package contract and examples | Entry point or contract under the directory named by the note: `apps/cli`. | `named-directory-member` |
| [`apps/cli/package.json`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/cli/package.json) | composition and configuration | Entry point or contract under the directory named by the note: `apps/cli`. | `named-directory-member` |
| [`apps/cli`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/apps/cli) | package or module directory | The source note names this implementation area directly. | `named-directory` |
| [`packages/interaction/commands`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/packages/interaction/commands) | package or module directory | The note names this package or capability. | `named-package` |
| [`packages/fs/tool-fs/src/edit.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/tool-fs/src/edit.ts) | runtime implementation | Defines `input`, a construct named by the note. | `symbol-definition` |
| [`packages/core/session/src/types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/types.ts) | public types and contract | Defines `SessionId`, a construct named by the note. | `symbol-definition` |
| [`packages/skill/tool-skill/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/skill/tool-skill/src/index.ts) | package entry point | Defines `skills`, a construct named by the note. | `symbol-definition` |
| [`packages/hooks/hooks-codex/src/config.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/hooks/hooks-codex/src/config.ts) | runtime implementation | Defines `commands`, a construct named by the note. | `symbol-definition` |
| [`packages/hooks/hook-protocol/src/merge.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/hooks/hook-protocol/src/merge.ts) | runtime implementation | Defines `list`, a construct named by the note. | `symbol-definition` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `CommandDirectory` | `class` | [`packages/client/ui-commands/src/client/directory.ts:34`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-commands/src/client/directory.ts#L34) | `export class CommandDirectory {` |
| `PopupSelectController` | `class` | [`packages/client/ui-commands/src/client/popup.ts:116`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-commands/src/client/popup.ts#L116) | `export class PopupSelectController<TCtx = unknown> {` |
| `CommandUiRuntime` | `class` | [`packages/client/ui-commands/src/client/service.ts:120`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-commands/src/client/service.ts#L120) | `export class CommandUiRuntime extends Service implements CommandUiContract {` |
| `lexicon` | `const` | [`packages/client/ui-conversation/src/client/skeleton/InputBar.tsx:56`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/src/client/skeleton/InputBar.tsx#L56) | `const lexicon = useLexicon(s => s)` |
| `SessionId` | `type` | [`packages/core/session/src/types.ts:22`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/types.ts#L22) | `export type SessionId = Branded<'SessionId'>` |
| `input` | `const` | [`packages/fs/tool-fs/src/edit.ts:113`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/tool-fs/src/edit.ts#L113) | `const input = parseEditArgs(args)` |
| `list` | `const` | [`packages/hooks/hook-protocol/src/merge.ts:75`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/hooks/hook-protocol/src/merge.ts#L75) | `const list = reasonsByRank.get(r) ?? []` |
| `commands` | `const` | [`packages/hooks/hooks-codex/src/config.ts:60`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/hooks/hooks-codex/src/config.ts#L60) | `const commands: MatcherGroup['hooks'] = []` |
| `skills` | `const` | [`packages/skill/tool-skill/src/index.ts:226`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/skill/tool-skill/src/index.ts#L226) | `const skills = snapshot.skills.filter(isModelInvocable)` |
| `available` | `const` | [`packages/subagent/subagent-codex/src/wire.ts:32`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/subagent/subagent-codex/src/wire.ts#L32) | `const available = params.availableDecisions` |

### Tests and executable evidence

- [`packages/client/ui-commands/tests/popup.client.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-commands/tests/popup.client.spec.ts) — A test under the owning area exercises or imports `PopupSelectController`.
- [`packages/client/ui-commands/tests/service.client.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-commands/tests/service.client.spec.ts) — A test under the owning area exercises or imports `CommandUiRuntime`.
- [`packages/client/ui-commands/tests/directory.client.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-commands/tests/directory.client.spec.ts) — A test under the owning area exercises or imports `CommandDirectory`.
- [`packages/client/ui-commands/tests/popup-view.client.spec.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-commands/tests/popup-view.client.spec.tsx) — A test under the owning area exercises or imports `PopupSelectController`.
- [`packages/client/ui-conversation/tests/skeleton.client.spec.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/tests/skeleton.client.spec.tsx) — A test under the owning area exercises or imports `lexicon`.
- [`packages/client/ui-commands/tests/browser-plugin.client.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-commands/tests/browser-plugin.client.spec.ts) — A test under the owning area exercises or imports `CommandUiRuntime`.
- [`packages/client/ui-conversation/tests/input-bar.client.spec.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/tests/input-bar.client.spec.tsx) — A test under the owning area exercises or imports `lexicon`.
- [`packages/client/ui-conversation/tests/apply-inject.client.spec.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/tests/apply-inject.client.spec.tsx) — A test under the owning area exercises or imports `lexicon`.

## How to read the implementation

1. Start with [`packages/interaction/commands/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/interaction/commands/src/index.ts) because it has the strongest evidence link to the note.
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
- **`rust/context-evidence`** — Compaction may shrink presentation, but it must not erase the evidence ledger, file activity, or durable session facts.
- **`rust/four-tool-surface`** — Preserve the four model tools: read, write, patch, and exec. Reuse internal capabilities without automatically exposing new model-facing tools.
- **`rust/newtype-ids`** — Represent identity domains with Rust newtypes; do not pass bare strings across session, call, turn, step, and agent boundaries.
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

- Tags: `class/architecture`, `concern/boundary`, `concern/concurrency`, `concern/discovery-routing`, `concern/lifecycle`, `concern/ownership`, `concern/performance`, `concern/recovery`, `domain/context`, `domain/extensions`, `domain/filesystem`, `domain/llm`, `domain/multi-agent`, `domain/session-state`, `domain/shell-terminal`, `domain/storage`, `domain/testing`, `lifecycle/implemented`, `mechanism/generation`, `mechanism/policy`, `mechanism/projection`, `mechanism/registry`
- Aliases: `CommandDirectory`, `PopupSelectController`, `CommandUiRuntime`, `lexicon`, `SessionId`, `input`, `list`, `commands`, `skills`, `available`, `ctx.commands`, `ctx.skills`, `ClientSessionContext { sessionId }`, `{sessionId}`
- Regex: `(?i)(CommandDirectory|PopupSelectController|CommandUiRuntime|lexicon|SessionId|input|list|commands)`

```bash
rg -n --pcre2 "(?i)(CommandDirectory|PopupSelectController|CommandUiRuntime|lexicon|SessionId|input|list|commands)" source-deepseek-harness
rg -l --fixed-strings "class/architecture" 03-agent-context
```

## Connected notes

- **`source-link`** — [0061. Web client Agent-scope parity model and the provisioning channel (agents/scope / blank reuse / provide)](0061-web-client-agent-scope-parity-model-and-the-provisioning-channel-agents.md): The source note links to this decision directly.
- **`source-link`** — [0063. Web input state machine, composer slots, and the slash pipeline (ui-conversation input / ui-input-trigger)](0063-web-input-state-machine-composer-slots-and-the-slash-pipeline-ui-convers.md): The source note links to this decision directly.
- **`shares-code-with`** — [0215. Queued manual compaction with one durable lock](0215-queued-manual-compaction-with-one-durable-lock.md): Shares source implementation: `packages/interaction/commands`, `packages/interaction/commands/src/index.ts`.
- **`shares-code-with`** — [0475. Remove the TUI package](0475-remove-the-tui-package.md): Shares source implementation: `apps/cli`, `packages/interaction/commands`.
- **`shares-code-with`** — [0579. Product-level TUI session resume](0579-product-level-tui-session-resume.md): Shares source implementation: `apps/cli`, `apps/cli/README.md`.
- **`shares-code-with`** — [0163. Plugin-owned human command registration](0163-plugin-owned-human-command-registration.md): Shares source implementation: `packages/interaction/commands`, `packages/interaction/commands/src/index.ts`.
- **`shares-code-with`** — [0551. Effect-owned TUI interactive extensions](0551-effect-owned-tui-interactive-extensions.md): Shares source implementation: `packages/interaction/commands`, `packages/interaction/commands/src/index.ts`.
- **`shares-code-with`** — [0160. Human `/goal` command](0160-human-goal-command.md): Shares source implementation: `packages/interaction/commands/src/index.ts`, `packages/interaction/commands/src/invariant.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0062-web-command-business-surfaces-and-assembly-ui-commands-ui-skill-ui-subag.md`.
