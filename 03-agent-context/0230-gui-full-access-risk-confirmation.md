---
id: "dsh-note-0230"
title: "GUI Full access risk confirmation"
status: "implemented"
class: "feature"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/feature/2026-07-31-gui-full-access-confirmation.md"
implementation_evidence: "medium"
target_anchor: "sandbox capability boundary"
tags:
  - "class/feature"
  - "concern/boundary"
  - "concern/cancellation-timeout"
  - "concern/lifecycle"
  - "concern/trust"
  - "domain/configuration"
  - "domain/context"
  - "domain/security"
  - "domain/session-state"
  - "domain/shell-terminal"
  - "domain/ui-interaction"
  - "domain/web-retrieval"
  - "lifecycle/implemented"
  - "mechanism/policy"
aliases:
  - "conversation"
  - "PopupSelectView"
  - "confirmation"
  - "SelectOption"
  - "locked"
  - "PermissionSelect"
  - "Modal"
  - "RiskConfirmation"
  - "command"
  - "danger-full-access"
  - "Danger Full Access"
  - "contentClassName"
  - "/permission"
  - "/permission danger-full-access"
search_regex: "(?i)(conversation|PopupSelectView|confirmation|SelectOption|locked|PermissionSelect|Modal|RiskConfirmation)"
---

# 0230. GUI Full access risk confirmation — implementation context

## Open this when

Switching the web client to danger-full-access was a single click on a permission picker, with the preset shown as the title-cased machine name Danger Full Access. Full access reduces confirmation steps and lets the agent run sensitive operations, modify files, or execute external commands, so an accidental pick armed the most dangerous preset with no deliberate acknowledgement step.

## Source decision

Every permission picker gates danger-full-access behind the shared in-page RiskConfirmation dialog whose enabling action stays disabled until an explicit acknowledgement checkbox is checked; the preset renders under the product label Full access; every dismissal path submits nothing. RiskConfirmation (ui-primitives) is a controlled Modal composition: title, description, acknowledgement checkbox, cancel, and a confirm button disabled until acknowledged. It stays an in-page dialog --- the Modal portals to this document's body and never opens a native or separate browser window that could land on another display.

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/feature/2026-07-31-gui-full-access-confirmation.md](../02-notes/implemented/feature/2026-07-31-gui-full-access-confirmation.md)
- Pinned source: [.agents/notes/implemented/feature/2026-07-31-gui-full-access-confirmation.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/feature/2026-07-31-gui-full-access-confirmation.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`packages/goal/command-goal/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/goal/command-goal/src/index.ts) | package entry point | Defines `command`, a construct named by the note. | `symbol-definition` |
| [`packages/hooks/hooks-codex/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/hooks/hooks-codex/src/index.ts) | package entry point | Defines `command`, a construct named by the note. | `symbol-definition` |
| [`packages/interaction/commands/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/interaction/commands/src/index.ts) | package entry point | Defines `command`, a construct named by the note. | `symbol-definition` |
| [`packages/context/tmux-context/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/context/tmux-context/src/index.ts) | package entry point | Defines `command`, a construct named by the note. | `symbol-definition` |
| [`packages/client/ui-primitives/src/Modal.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-primitives/src/Modal.tsx) | runtime implementation | Defines `Modal`, a construct named by the note. | `symbol-definition` |
| [`packages/e2b/subprocess-e2b/src/terminal.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/e2b/subprocess-e2b/src/terminal.ts) | runtime implementation | Defines `command`, a construct named by the note. | `symbol-definition` |
| [`packages/client/runtime/src/client/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/runtime/src/client/index.ts) | package entry point | Defines `conversation`, a construct named by the note. | `symbol-definition` |
| [`packages/client/ui-commands/src/client/service.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-commands/src/client/service.ts) | runtime implementation | Defines `conversation`, a construct named by the note. | `symbol-definition` |
| [`packages/client/ui-commands/src/client/contract.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-commands/src/client/contract.ts) | runtime implementation | Defines `SelectOption`, a construct named by the note. | `symbol-definition` |
| [`packages/client/ui-conversation/src/client/apply.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/src/client/apply.ts) | runtime implementation | Defines `conversation`, a construct named by the note. | `symbol-definition` |
| [`packages/interaction/permission-presets/src/types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/interaction/permission-presets/src/types.ts) | public types and contract | Defines `PermissionSelect`, a construct named by the note. | `symbol-definition` |
| [`packages/client/ui-primitives/src/RiskConfirmation.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-primitives/src/RiskConfirmation.tsx) | runtime implementation | Defines `RiskConfirmation`, a construct named by the note. | `symbol-definition` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `conversation` | `const` | [`packages/client/runtime/src/client/index.ts:190`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/runtime/src/client/index.ts#L190) | `const conversation = {` |
| `PopupSelectView` | `function` | [`packages/client/ui-commands/src/client/PopupSelectView.tsx:38`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-commands/src/client/PopupSelectView.tsx#L38) | `export function PopupSelectView({ popup, t }: PopupSelectViewProps) {` |
| `confirmation` | `const` | [`packages/client/ui-commands/src/client/PopupSelectView.tsx:80`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-commands/src/client/PopupSelectView.tsx#L80) | `const confirmation = state.confirming?.confirmation` |
| `SelectOption` | `interface` | [`packages/client/ui-commands/src/client/contract.ts:19`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-commands/src/client/contract.ts#L19) | `export interface SelectOption {` |
| `conversation` | `const` | [`packages/client/ui-commands/src/client/service.ts:439`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-commands/src/client/service.ts#L439) | `const conversation = actx.get('conversation')` |
| `conversation` | `const` | [`packages/client/ui-conversation/src/client/apply.ts:95`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/src/client/apply.ts#L95) | `const conversation = scoped.get('conversation')` |
| `conversation` | `const` | [`packages/client/ui-conversation/src/client/apply.ts:102`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/src/client/apply.ts#L102) | `const conversation = ctx.get('conversation') as ConversationController \| undefined` |
| `conversation` | `const` | [`packages/client/ui-conversation/src/client/apply.ts:246`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/src/client/apply.ts#L246) | `const conversation = concreteConversation(ctx)` |
| `locked` | `const` | [`packages/client/ui-conversation/src/client/skeleton/InputBar.tsx:134`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/src/client/skeleton/InputBar.tsx#L134) | `const locked = disabled` |
| `PermissionSelect` | `function` | [`packages/client/ui-conversation/src/client/skeleton/PermissionSelect.tsx:72`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/src/client/skeleton/PermissionSelect.tsx#L72) | `export function PermissionSelect({ value, locked, command, t }: PermissionSelectProps) {` |
| `Modal` | `function` | [`packages/client/ui-primitives/src/Modal.tsx:30`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-primitives/src/Modal.tsx#L30) | `export function Modal({` |
| `RiskConfirmation` | `function` | [`packages/client/ui-primitives/src/RiskConfirmation.tsx:28`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-primitives/src/RiskConfirmation.tsx#L28) | `export function RiskConfirmation({` |
| `command` | `const` | [`packages/context/tmux-context/src/index.ts:114`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/context/tmux-context/src/index.ts#L114) | `const command = [` |
| `command` | `const` | [`packages/e2b/subprocess-e2b/src/terminal.ts:514`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/e2b/subprocess-e2b/src/terminal.ts#L514) | `const command = \`exec /bin/bash ${quoteE2BShellArg(paths.runner)} ${quoteE2BShellArg(stateDir)}\r\`` |
| `command` | `const` | [`packages/goal/command-goal/src/index.ts:111`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/goal/command-goal/src/index.ts#L111) | `const command = parseGoalCommand(invocation.rawInput)` |
| `command` | `const` | [`packages/hooks/hooks-codex/src/index.ts:313`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/hooks/hooks-codex/src/index.ts#L313) | `const command: unknown = args.command` |

### Tests and executable evidence

- [`scripts/publint-all.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/publint-all.spec.ts) — A test under the owning area exercises or imports `tsx`.
- [`apps/web/tests/goal-bar.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/goal-bar.e2e.ts) — A test under the owning area exercises or imports `acknowledged`.
- [`scripts/coverage-exempt.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/coverage-exempt.spec.ts) — A test under the owning area exercises or imports `tsx`.
- [`scripts/install-lefthook.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/install-lefthook.spec.ts) — A test under the owning area exercises or imports `tsx`.
- [`scripts/client-bundle-purity.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/client-bundle-purity.spec.ts) — A test under the owning area exercises or imports `tsx`.
- [`apps/web/tests/settings-chrome.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/settings-chrome.e2e.ts) — A test under the owning area exercises or imports `danger-full-access`.
- [`apps/web/tests/skill-user-invoke.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/skill-user-invoke.e2e.ts) — A test under the owning area exercises or imports `acknowledged`.
- [`scripts/gen-third-party-notices.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/gen-third-party-notices.spec.ts) — A test under the owning area exercises or imports `tsx`.

## How to read the implementation

1. Start with [`packages/goal/command-goal/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/goal/command-goal/src/index.ts) because it has the strongest evidence link to the note.
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
- **`rust/newtype-ids`** — Represent identity domains with Rust newtypes; do not pass bare strings across session, call, turn, step, and agent boundaries.
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

- Tags: `class/feature`, `concern/boundary`, `concern/cancellation-timeout`, `concern/lifecycle`, `concern/trust`, `domain/configuration`, `domain/context`, `domain/security`, `domain/session-state`, `domain/shell-terminal`, `domain/ui-interaction`, `domain/web-retrieval`, `lifecycle/implemented`, `mechanism/policy`
- Aliases: `conversation`, `PopupSelectView`, `confirmation`, `SelectOption`, `locked`, `PermissionSelect`, `Modal`, `RiskConfirmation`, `command`, `danger-full-access`, `Danger Full Access`, `contentClassName`, `/permission`, `/permission danger-full-access`
- Regex: `(?i)(conversation|PopupSelectView|confirmation|SelectOption|locked|PermissionSelect|Modal|RiskConfirmation)`

```bash
rg -n --pcre2 "(?i)(conversation|PopupSelectView|confirmation|SelectOption|locked|PermissionSelect|Modal|RiskConfirmation)" source-deepseek-harness
rg -l --fixed-strings "class/feature" 03-agent-context
```

## Connected notes

- **`shares-code-with`** — [0160. Human `/goal` command](0160-human-goal-command.md): Shares source implementation: `packages/goal/command-goal/src/index.ts`, `packages/interaction/commands/src/index.ts`.
- **`shares-code-with`** — [0199. Workspace Registration Deletion](0199-workspace-registration-deletion.md): Shares source implementation: `packages/client/runtime/src/client/index.ts`, `packages/client/ui-primitives/src/Modal.tsx`.
- **`shares-code-with`** — [0215. Queued manual compaction with one durable lock](0215-queued-manual-compaction-with-one-durable-lock.md): Shares source implementation: `packages/interaction/commands/src/index.ts`.
- **`shares-code-with`** — [0193. tmux-location context](0193-tmux-location-context.md): Shares source implementation: `packages/context/tmux-context/src/index.ts`.
- **`shares-code-with`** — [0161. Model-facing same-session goal tools](0161-model-facing-same-session-goal-tools.md): Shares source implementation: `packages/goal/command-goal/src/index.ts`.
- **`shares-code-with`** — [0157. Harness-level goal-based execution](0157-harness-level-goal-based-execution.md): Shares source implementation: `packages/goal/command-goal/src/index.ts`.
- **`shares-code-with`** — [0139. dsh-hook-protocol --- the shared Claude Code / Codex hook wire-protocol core](0139-dsh-hook-protocol-the-shared-claude-code-codex-hook-wire-protocol-core.md): Shares source implementation: `packages/hooks/hooks-codex/src/index.ts`.
- **`shares-code-with`** — [0080. Full client copy rollout onto the typed locale seat, and the non-translation boundary](0080-full-client-copy-rollout-onto-the-typed-locale-seat-and-the-non-translat.md): Shares source implementation: `packages/client/ui-primitives/src/Modal.tsx`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0230-gui-full-access-risk-confirmation.md`.
