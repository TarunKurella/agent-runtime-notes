---
id: "dsh-note-0081"
title: "Command row copy is split between the row and the handler"
status: "implemented"
class: "architecture"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/architecture/2026-07-30-command-row-copy-contract.md"
implementation_evidence: "medium"
target_anchor: "sandbox capability boundary"
tags:
  - "class/architecture"
  - "concern/evidence"
  - "concern/lifecycle"
  - "concern/ownership"
  - "concern/recovery"
  - "concern/trust"
  - "domain/configuration"
  - "domain/filesystem"
  - "domain/security"
  - "domain/session-state"
  - "domain/storage"
  - "domain/ui-interaction"
  - "lifecycle/implemented"
  - "mechanism/policy"
  - "mechanism/projection"
aliases:
  - "args"
  - "command/run"
  - "/permission workspace-write"
  - "command/done"
  - "Permission preset: workspace-write."
  - ", no arguments. The"
  - "fallback for a cross-window node whose"
  - "/permission"
  - "preset workspace-write"
  - "current preset workspace-write"
  - "permission · preset workspace-write"
  - "/plan"
  - "Plan mode off."
  - "Plan mode on. Use /plan off to leave."
search_regex: "(?i)(args|command/run|/permission[- ]workspace\\-write|command/done|Permission[- ]preset:[- ]workspace\\-write\\.|,[- ]no[- ]arguments\\.[- ]The|fallback[- ]for[- ]a[- ]cross\\-window[- ]node[- ]whose|/permission)"
---

# 0081. Command row copy is split between the row and the handler — implementation context

## Open this when

The web command row renders title · summary from one logged command lifecycle pair: the title was the dispatched line rebuilt from command/run (/permission workspace-write) and the summary was command/done's verbatim text (Permission preset: workspace-write.). Both halves were written without knowing about the other, so the row said the command name twice and its argument twice --- the single worst case being the row a user gets for every Access-chip pick.

## Source decision

The row's two halves have disjoint jobs, and each side is written to its own half alone. The row title is the bare command name --- no /, no arguments. The / belongs to the composer's input grammar, not to a settled record, and the argument is not the row's to report: the summary already says what the command did. GenericCommandCard keeps the 命令 fallback for a cross-window node whose command/run page fell out of the client's window. A command handler's settlement text therefore never labels its value with the command's own name, because the surface that renders it has already said it.

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/architecture/2026-07-30-command-row-copy-contract.md](../02-notes/implemented/architecture/2026-07-30-command-row-copy-contract.md)
- Pinned source: [.agents/notes/implemented/architecture/2026-07-30-command-row-copy-contract.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/architecture/2026-07-30-command-row-copy-contract.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`vendor/cordis/src/fiber.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/cordis/src/fiber.ts) | runtime implementation | Defines `args`, a construct named by the note. | `symbol-definition` |
| [`vendor/cordis/src/logger.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/cordis/src/logger.ts) | runtime implementation | Defines `args`, a construct named by the note. | `symbol-definition` |
| [`packages/core/agent/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent/src/index.ts) | package entry point | Defines `args`, a construct named by the note. | `symbol-definition` |
| [`packages/api/gateway/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/api/gateway/src/index.ts) | package entry point | Defines `args`, a construct named by the note. | `symbol-definition` |
| [`docs/persistence-catalog.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/persistence-catalog.md) | package contract and examples | Contains the exact code literal `command/run` named by the note. Contains the exact code literal `command/done` named by the note. | `exact-code-occurrence` |
| [`docs/persistence-catalog.zh.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/persistence-catalog.zh.md) | package contract and examples | Contains the exact code literal `command/run` named by the note. Contains the exact code literal `command/done` named by the note. | `exact-code-occurrence` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `args` | `const` | [`packages/api/gateway/src/index.ts:159`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/api/gateway/src/index.ts#L159) | `const args = await Promise.all(descriptor.parameters.map(parameter =>` |
| `args` | `const` | [`packages/core/agent/src/index.ts:529`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent/src/index.ts#L529) | `const args: unknown[] = [entry.carrier, 'agent/disposed', { agent: entry.agent }]` |
| `args` | `const` | [`packages/core/agent/src/index.ts:561`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent/src/index.ts#L561) | `const args: unknown[] = [entry.carrier, 'agent/created', { agent: entry.agent }]` |
| `args` | `const` | [`vendor/cordis/src/fiber.ts:121`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/cordis/src/fiber.ts#L121) | `const args: any[] = ['internal/plugin', fiber]` |
| `args` | `const` | [`vendor/cordis/src/logger.ts:100`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/cordis/src/logger.ts#L100) | `const args = message.args.slice()` |

### Tests and executable evidence

- [`apps/web/tests/snapshots/seeded-history/command-row.expected.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/snapshots/seeded-history/command-row.expected.md) — The source note names this file directly.
- [`apps/web/tests/seeded-history.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/seeded-history.e2e.ts) — Contains the exact code literal `command/run` named by the note. Contains the exact code literal `command/done` named by the note.
- [`apps/web/tests/navigation-panes.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/navigation-panes.e2e.ts) — Contains the exact code literal `command/run` named by the note. Contains the exact code literal `command/done` named by the note.
- [`apps/web/tests/feedback-command.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/feedback-command.e2e.ts) — Contains the exact code literal `command/run` named by the note. Contains the exact code literal `command/done` named by the note.
- [`apps/web/tests/goal-command-presentation.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/goal-command-presentation.e2e.ts) — Contains the exact code literal `command/run` named by the note. Contains the exact code literal `command/done` named by the note.
- [`apps/web/tests/snapshots/approval-composer/session.jsonl`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/snapshots/approval-composer/session.jsonl) — Contains the exact code literal `command/run` named by the note. Contains the exact code literal `command/done` named by the note.
- [`apps/web/tests/snapshots/permission-policy-context/session.jsonl`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/snapshots/permission-policy-context/session.jsonl) — Contains the exact code literal `command/run` named by the note. Contains the exact code literal `command/done` named by the note.

## How to read the implementation

1. Start with [`vendor/cordis/src/fiber.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/cordis/src/fiber.ts) because it has the strongest evidence link to the note.
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
- **`rust/quickjs-boundary`** — QuickJS holds orchestration variables; Rust/Tokio owns async execution and exposes only explicitly granted capabilities.

### Smallest safe implementation slice

1. Express the semantic contract as Rust types and one owning boundary.
2. Add the smallest adapter or service needed by the existing loop; do not add a new model-facing tool unless the four-tool surface cannot express the job.
3. Record durable evidence before deriving summaries or UI state.
4. Add one happy-path test, one interruption test, and one replay/restart test where state survives a turn.
5. Keep the patch reversible until the source note's strongest failure case is proven in the Rust harness.

## Search handles

- Tags: `class/architecture`, `concern/evidence`, `concern/lifecycle`, `concern/ownership`, `concern/recovery`, `concern/trust`, `domain/configuration`, `domain/filesystem`, `domain/security`, `domain/session-state`, `domain/storage`, `domain/ui-interaction`, `lifecycle/implemented`, `mechanism/policy`, `mechanism/projection`
- Aliases: `args`, `command/run`, `/permission workspace-write`, `command/done`, `Permission preset: workspace-write.`, `, no arguments. The`, `fallback for a cross-window node whose`, `/permission`, `preset workspace-write`, `current preset workspace-write`, `permission · preset workspace-write`, `/plan`, `Plan mode off.`, `Plan mode on. Use /plan off to leave.`
- Regex: `(?i)(args|command/run|/permission[- ]workspace\-write|command/done|Permission[- ]preset:[- ]workspace\-write\.|,[- ]no[- ]arguments\.[- ]The|fallback[- ]for[- ]a[- ]cross\-window[- ]node[- ]whose|/permission)`

```bash
rg -n --pcre2 "(?i)(args|command/run|/permission[- ]workspace\\-write|command/done|Permission[- ]preset:[- ]workspace\\-write\\.|,[- ]no[- ]arguments\\.[- ]The|fallback[- ]for[- ]a[- ]cross\\-window[- ]node[- ]whose|/permission)" source-deepseek-harness
rg -l --fixed-strings "class/architecture" 03-agent-context
```

## Connected notes

- **`source-link`** — [0511. Session projections and command lifecycle logging](0511-session-projections-and-command-lifecycle-logging.md): The source note links to this decision directly.
- **`shares-code-with`** — [0092. Typert Gateway Targeted Method Calls](0092-typert-gateway-targeted-method-calls.md): Shares source implementation: `packages/api/gateway/src/index.ts`, `packages/core/agent/src/index.ts`.
- **`shares-code-with`** — [0093. A session's agent is composed from a preset cordis.yml](0093-a-session-s-agent-is-composed-from-a-preset-cordis-yml.md): Shares source implementation: `packages/api/gateway/src/index.ts`, `vendor/cordis/src/fiber.ts`.
- **`shares-code-with`** — [0121. Remote event delivery (ctx.remote.$on)](0121-remote-event-delivery-ctx-remote-on.md): Shares source implementation: `vendor/cordis/src/fiber.ts`, `vendor/cordis/src/logger.ts`.
- **`shares-code-with`** — [0597. `dsh meta` boots the TUI over the harness checkout](0597-dsh-meta-boots-the-tui-over-the-harness-checkout.md): Shares source implementation: `vendor/cordis/src/fiber.ts`, `vendor/cordis/src/logger.ts`.
- **`shares-code-with`** — [0355. Broken presets are roster rows, not gaps](0355-broken-presets-are-roster-rows-not-gaps.md): Shares source implementation: `vendor/cordis/src/fiber.ts`, `vendor/cordis/src/logger.ts`.
- **`shares-code-with`** — [0601. Live standalone compaction progress in the terminal](0601-live-standalone-compaction-progress-in-the-terminal.md): Shares source implementation: `docs/persistence-catalog.md`, `docs/persistence-catalog.zh.md`.
- **`shares-code-with`** — [0144. The approval seam --- one-shot permission decisions over a waterfall of answerers](0144-the-approval-seam-one-shot-permission-decisions-over-a-waterfall-of-answ.md): Shares source implementation: `packages/core/agent/src/index.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0081-command-row-copy-is-split-between-the-row-and-the-handler.md`.
