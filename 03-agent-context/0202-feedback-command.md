---
id: "dsh-note-0202"
title: "`/feedback` command"
status: "implemented"
class: "feature"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/feature/2026-07-28-feedback-command.md"
implementation_evidence: "medium"
target_anchor: "Rust provider-adapter boundary"
tags:
  - "class/feature"
  - "concern/boundary"
  - "concern/discovery-routing"
  - "concern/evidence"
  - "concern/human-control"
  - "concern/lifecycle"
  - "concern/ownership"
  - "concern/recovery"
  - "domain/build-release"
  - "domain/configuration"
  - "domain/extensions"
  - "domain/llm"
  - "domain/observability"
  - "domain/protocols"
  - "domain/session-state"
  - "domain/storage"
  - "domain/testing"
  - "domain/tools"
  - "domain/ui-interaction"
  - "lifecycle/implemented"
  - "mechanism/adapter"
  - "mechanism/event-sourcing"
  - "mechanism/policy"
  - "mechanism/registry"
aliases:
  - "dsh"
  - "surfaceOp"
  - "SurfaceEventType"
  - "recordFeedback"
  - "commands"
  - "feedback"
  - "args"
  - "/feedback"
  - "@deepseek-ai/dsh-command-feedback"
  - "packages/feedback/command-feedback/"
  - "ctx.commands"
  - "/feedback <text>"
  - "$DSH_HOME/.anonymous-user-id"
  - "feedback/record { text }"
search_regex: "(?i)(surfaceOp|SurfaceEventType|recordFeedback|commands|feedback|args|/feedback|@deepseek\\-ai/dsh\\-command\\-feedback)"
---

# 0202. `/feedback` command — implementation context

## Open this when

A user who notices something wrong mid-session has nowhere to put that observation. Telling the model wastes a turn, changes the conversation the user was having, and buries the remark in derived history where no later reader can find it. Writing it outside the session loses the context that makes it meaningful --- which session, at which point, against which work.

## Source decision

@deepseek-ai/dsh-command-feedback in packages/feedback/command-feedback/ registers one global feedback command over ctx.commands. /feedback acknowledges with the receiving session id and the shared harness-home anonymous user id; bare or whitespace-only input returns a direct usage error. The handler is synchronous, injects only commands, and has no configuration. The shared-id decision records why feedback and OpenTelemetry use the same $DSH_HOME/.anonymous-user-id value.

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/feature/2026-07-28-feedback-command.md](../02-notes/implemented/feature/2026-07-28-feedback-command.md)
- Pinned source: [.agents/notes/implemented/feature/2026-07-28-feedback-command.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/feature/2026-07-28-feedback-command.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`packages/interaction/commands/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/interaction/commands/src/index.ts) | package entry point | Entry point or contract under the directory named by the note: `packages/interaction/commands`. Core file in the package named by the note: `packages/interaction/commands`. | `named-directory-member, named-package-member` |
| [`packages/interaction/commands/src/types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/interaction/commands/src/types.ts) | public types and contract | Entry point or contract under the directory named by the note: `packages/interaction/commands`. Core file in the package named by the note: `packages/interaction/commands`. | `named-directory-member, named-package-member` |
| [`packages/interaction/commands/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/interaction/commands/src/invariant.ts) | runtime contract checks | Entry point or contract under the directory named by the note: `packages/interaction/commands`. Core file in the package named by the note: `packages/interaction/commands`. | `named-directory-member, named-package-member` |
| [`packages/feedback/command-feedback/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/feedback/command-feedback/src/index.ts) | package entry point | Entry point or contract under the directory named by the note: `packages/feedback`. Entry point or contract under the directory named by the note: `packages/feedback/command-feedback`. | `named-directory-member, named-package-member, symbol-definition` |
| [`packages/feedback/command-feedback/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/feedback/command-feedback/src/invariant.ts) | runtime contract checks | Entry point or contract under the directory named by the note: `packages/feedback/command-feedback`. Core file in the package named by the note: `packages/feedback/command-feedback`. | `named-directory-member, named-package-member` |
| [`packages/feedback/README.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/feedback/README.md) | package contract and examples | Entry point or contract under the directory named by the note: `packages/feedback`. Contains the exact code literal `feedback/record` named by the note. | `exact-code-occurrence, named-directory-member` |
| [`packages/interaction/commands/README.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/interaction/commands/README.md) | package contract and examples | Entry point or contract under the directory named by the note: `packages/interaction/commands`. Core file in the package named by the note: `packages/interaction/commands`. | `named-directory-member, named-package-member` |
| [`packages/interaction/commands/package.json`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/interaction/commands/package.json) | composition and configuration | Entry point or contract under the directory named by the note: `packages/interaction/commands`. Core file in the package named by the note: `packages/interaction/commands`. | `named-directory-member, named-package-member` |
| [`packages/feedback/command-feedback/README.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/feedback/command-feedback/README.md) | package contract and examples | Entry point or contract under the directory named by the note: `packages/feedback/command-feedback`. Core file in the package named by the note: `packages/feedback/command-feedback`. | `named-directory-member, named-package-member` |
| [`packages/feedback/command-feedback/package.json`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/feedback/command-feedback/package.json) | composition and configuration | Entry point or contract under the directory named by the note: `packages/feedback/command-feedback`. Core file in the package named by the note: `packages/feedback/command-feedback`. | `named-directory-member, named-package-member` |
| [`packages/feedback`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/packages/feedback) | package or module directory | The source note names this implementation area directly. | `named-directory` |
| [`packages/interaction/commands`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/packages/interaction/commands) | package or module directory | The source note names this implementation area directly. The note names this package or capability. | `named-directory, named-package` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `dsh` | `const` | [`packages/client/modules/src/index.ts:345`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/modules/src/index.ts#L345) | `const dsh = pkg.dsh` |
| `surfaceOp` | `const` | [`packages/core/session/src/surface.ts:331`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/surface.ts#L331) | `const surfaceOp = surfaceOpOf(event)` |
| `SurfaceEventType` | `type` | [`packages/core/session/src/types.ts:343`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/types.ts#L343) | `export type SurfaceEventType =` |
| `recordFeedback` | `function` | [`packages/feedback/command-feedback/src/index.ts:72`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/feedback/command-feedback/src/index.ts#L72) | `export function recordFeedback(session: Session, text: string): void {` |
| `commands` | `const` | [`packages/hooks/hooks-codex/src/config.ts:60`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/hooks/hooks-codex/src/config.ts#L60) | `const commands: MatcherGroup['hooks'] = []` |
| `feedback` | `const` | [`packages/plan/plan-mode/src/index.ts:371`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/plan/plan-mode/src/index.ts#L371) | `const feedback = item?.custom ?? ''` |
| `args` | `const` | [`vendor/cordis/src/fiber.ts:121`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/cordis/src/fiber.ts#L121) | `const args: any[] = ['internal/plugin', fiber]` |

### Tests and executable evidence

- [`packages/core/session/tests/surface.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/tests/surface.spec.ts) — A test under the owning area exercises or imports `SurfaceEventType`.
- [`packages/core/session/tests/gen-persistence-catalog.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/tests/gen-persistence-catalog.spec.ts) — A test under the owning area exercises or imports `SurfaceEventType`.
- [`packages/feedback/command-feedback/tests/command-feedback.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/feedback/command-feedback/tests/command-feedback.spec.ts) — A test under the owning area exercises or imports `recordFeedback`. A test under the owning area exercises or imports `dsh-commands`.
- [`packages/feedback/command-feedback/tests/loader-composition.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/feedback/command-feedback/tests/loader-composition.spec.ts) — A test under the owning area exercises or imports `dsh-commands`.
- [`apps/web/tests/seeded-history.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/seeded-history.e2e.ts) — Contains the exact code literal `command/run` named by the note. Contains the exact code literal `command/done` named by the note.
- [`apps/web/tests/navigation-panes.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/navigation-panes.e2e.ts) — Contains the exact code literal `command/run` named by the note. Contains the exact code literal `command/done` named by the note.
- [`apps/web/tests/feedback-command.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/feedback-command.e2e.ts) — Contains the exact code literal `command/run` named by the note. Contains the exact code literal `command/done` named by the note.
- [`apps/web/tests/shipped-composition.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/shipped-composition.e2e.ts) — Contains the exact code literal `dsh-commands` named by the note.

## How to read the implementation

1. Start with [`packages/interaction/commands/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/interaction/commands/src/index.ts) because it has the strongest evidence link to the note.
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
- **`rust/trait-seam`** — Use a small trait at the capability boundary; keep policy and model-facing schemas outside the backend implementation.

### Smallest safe implementation slice

1. Express the semantic contract as Rust types and one owning boundary.
2. Add the smallest adapter or service needed by the existing loop; do not add a new model-facing tool unless the four-tool surface cannot express the job.
3. Record durable evidence before deriving summaries or UI state.
4. Add one happy-path test, one interruption test, and one replay/restart test where state survives a turn.
5. Keep the patch reversible until the source note's strongest failure case is proven in the Rust harness.

## Search handles

- Tags: `class/feature`, `concern/boundary`, `concern/discovery-routing`, `concern/evidence`, `concern/human-control`, `concern/lifecycle`, `concern/ownership`, `concern/recovery`, `domain/build-release`, `domain/configuration`, `domain/extensions`, `domain/llm`, `domain/observability`, `domain/protocols`, `domain/session-state`, `domain/storage`, `domain/testing`, `domain/tools`, `domain/ui-interaction`, `lifecycle/implemented`, `mechanism/adapter`, `mechanism/event-sourcing`, `mechanism/policy`, `mechanism/registry`
- Aliases: `dsh`, `surfaceOp`, `SurfaceEventType`, `recordFeedback`, `commands`, `feedback`, `args`, `/feedback`, `@deepseek-ai/dsh-command-feedback`, `packages/feedback/command-feedback/`, `ctx.commands`, `/feedback <text>`, `$DSH_HOME/.anonymous-user-id`, `feedback/record { text }`
- Regex: `(?i)(surfaceOp|SurfaceEventType|recordFeedback|commands|feedback|args|/feedback|@deepseek\-ai/dsh\-command\-feedback)`

```bash
rg -n --pcre2 "(?i)(surfaceOp|SurfaceEventType|recordFeedback|commands|feedback|args|/feedback|@deepseek\\-ai/dsh\\-command\\-feedback)" source-deepseek-harness
rg -l --fixed-strings "class/feature" 03-agent-context
```

## Connected notes

- **`source-link`** — [0109. Shared anonymous user id across telemetry, feedback, and DeepSeek requests](0109-shared-anonymous-user-id-across-telemetry-feedback-and-deepseek-requests.md): The source note links to this decision directly.
- **`source-link`** — [0258. Feedback-gated session telemetry](0258-feedback-gated-session-telemetry.md): The source note links to this decision directly.
- **`source-link`** — [0273. Feedback acknowledgement sharing disclosure](0273-feedback-acknowledgement-sharing-disclosure.md): The source note links to this decision directly.
- **`shares-code-with`** — [0163. Plugin-owned human command registration](0163-plugin-owned-human-command-registration.md): Shares source implementation: `packages/interaction/commands/README.md`, `packages/interaction/commands/package.json`.
- **`shares-code-with`** — [0551. Effect-owned TUI interactive extensions](0551-effect-owned-tui-interactive-extensions.md): Shares source implementation: `packages/interaction/commands/README.md`, `packages/interaction/commands/package.json`.
- **`shares-code-with`** — [0160. Human `/goal` command](0160-human-goal-command.md): Shares source implementation: `packages/interaction/commands/src/index.ts`, `packages/interaction/commands/src/invariant.ts`.
- **`shares-code-with`** — [0215. Queued manual compaction with one durable lock](0215-queued-manual-compaction-with-one-durable-lock.md): Shares source implementation: `packages/interaction/commands/src/index.ts`, `packages/interaction/commands/src/invariant.ts`.
- **`shares-code-with`** — [0475. Remove the TUI package](0475-remove-the-tui-package.md): Shares source implementation: `packages/interaction/commands/src/index.ts`, `packages/interaction/commands/src/invariant.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0202-feedback-command.md`.
