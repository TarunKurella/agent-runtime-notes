---
id: "dsh-note-0669"
title: "TUI titles come from the session-title service"
status: "archived"
class: "simplification"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/archived/simplification/2026-07-22-tui-titles-from-session-title-service.md"
implementation_evidence: "medium"
target_anchor: "Rust provider-adapter boundary"
tags:
  - "class/simplification"
  - "concern/boundary"
  - "concern/cancellation-timeout"
  - "concern/discovery-routing"
  - "concern/evidence"
  - "concern/ownership"
  - "concern/performance"
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
  - "mechanism/adapter"
  - "mechanism/event-sourcing"
  - "mechanism/generation"
  - "mechanism/policy"
aliases:
  - "llm"
  - "foldSessionTitle"
  - "SessionTitleService"
  - "autoTitle"
  - "runtime.terminal.setTitle"
  - "setTitle"
  - "<session title> --- <configured title>"
  - "session/title"
  - "examples/tui-agent/cordis.yml"
  - "@deepseek-ai/dsh-session-title-first-message-llm"
  - "dsh-agent-spine-demo"
  - "<title> --- <product>"
  - "TUI titles come from the session-title service"
  - "simplification"
search_regex: "(?i)(foldSessionTitle|SessionTitleService|autoTitle|runtime\\.terminal\\.setTitle|setTitle|<session[- ]title>[- ]\\-\\-\\-[- ]<configured[- ]title>|session/title|examples/tui\\-agent/cordis\\.yml)"
---

# 0669. TUI titles come from the session-title service — implementation context

## Open this when

A per-session title makes terminal panes and tabs distinguishable, but a TUI-local model call would create a second title pipeline beside log-backed session titles. The local path needs its own prompt, cap, one-shot latch, resume derivation, cancellation, and failure fallback, while its process-local result remains invisible to session listings, forks, Web consumers, and replay. If both paths run, one session can also be titled twice by different strategies.

## Source decision

The session-title service is the one title source. The TUI contains no autoTitle config, title-model request, latch, abort controller, prompt, or output cap. It folds the latest logged title on mount (foldSessionTitle), renders it as the banner subtitle, and calls runtime.terminal.setTitle with --- on every accepted session/title event. The same terminal-safe OSC 0 path handles the configured fallback title, resumed sessions, and live revisions without renaming tmux windows or adding another terminal-control surface.

## Decision status

Historical design. Use it to understand an older pressure or failure; locate the replacing decision before implementing anything.

- Raw note: [archived/simplification/2026-07-22-tui-titles-from-session-title-service.md](../02-notes/archived/simplification/2026-07-22-tui-titles-from-session-title-service.md)
- Pinned source: [.agents/notes/archived/simplification/2026-07-22-tui-titles-from-session-title-service.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/archived/simplification/2026-07-22-tui-titles-from-session-title-service.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`packages/llm/llm/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/src/index.ts) | package entry point | Core file in the package named by the note: `packages/llm/llm`. | `named-package-member` |
| [`packages/llm/llm/src/types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/src/types.ts) | public types and contract | Core file in the package named by the note: `packages/llm/llm`. | `named-package-member` |
| [`packages/llm/llm/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/llm/llm`. Defines `llm`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/examples/agent-spine-demo/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/examples/agent-spine-demo/src/index.ts) | package entry point | Core file in the package named by the note: `packages/examples/agent-spine-demo`. | `named-package-member` |
| [`packages/examples/agent-spine-demo/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/examples/agent-spine-demo/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/examples/agent-spine-demo`. | `named-package-member` |
| [`packages/llm/llm`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm) | package or module directory | The note names this package or capability. | `named-package` |
| [`packages/examples/agent-spine-demo`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/packages/examples/agent-spine-demo) | package or module directory | The note names this package or capability. | `named-package` |
| [`packages/session/session-title/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/session/session-title/src/index.ts) | package entry point | Defines `foldSessionTitle`, a construct named by the note. Defines `SessionTitleService`, a construct named by the note. | `symbol-definition` |
| [`packages/llm/llm/README.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/README.md) | package contract and examples | Core file in the package named by the note: `packages/llm/llm`. | `named-package-member` |
| [`packages/llm/llm/package.json`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/package.json) | composition and configuration | Core file in the package named by the note: `packages/llm/llm`. | `named-package-member` |
| [`packages/examples/agent-spine-demo/README.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/examples/agent-spine-demo/README.md) | package contract and examples | Core file in the package named by the note: `packages/examples/agent-spine-demo`. | `named-package-member` |
| [`packages/examples/agent-spine-demo/package.json`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/examples/agent-spine-demo/package.json) | composition and configuration | Core file in the package named by the note: `packages/examples/agent-spine-demo`. | `named-package-member` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `llm` | `const` | [`packages/llm/llm/src/invariant.ts:92`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/src/invariant.ts#L92) | `const llm = ctx.get('llm')` |
| `foldSessionTitle` | `function` | [`packages/session/session-title/src/index.ts:191`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/session/session-title/src/index.ts#L191) | `export function foldSessionTitle(events: readonly SessionEvent[]): SessionTitleSnapshot \| undefined {` |
| `SessionTitleService` | `class` | [`packages/session/session-title/src/index.ts:261`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/session/session-title/src/index.ts#L261) | `export class SessionTitleService extends Service {` |

### Tests and executable evidence

- [`packages/session/session-title/tests/rename.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/session/session-title/tests/rename.spec.ts) — A test under the owning area exercises or imports `foldSessionTitle`. A test under the owning area exercises or imports `SessionTitleService`.
- [`packages/session/session-title/tests/provider.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/session/session-title/tests/provider.spec.ts) — A test under the owning area exercises or imports `SessionTitleService`.
- [`packages/session/session-title/tests/projection.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/session/session-title/tests/projection.spec.ts) — A test under the owning area exercises or imports `foldSessionTitle`. A test under the owning area exercises or imports `SessionTitleService`.
- [`packages/session/session-title/tests/persistence.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/session/session-title/tests/persistence.spec.ts) — A test under the owning area exercises or imports `foldSessionTitle`. A test under the owning area exercises or imports `SessionTitleService`.
- [`packages/session/session-title/tests/session-title.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/session/session-title/tests/session-title.spec.ts) — A test under the owning area exercises or imports `foldSessionTitle`. A test under the owning area exercises or imports `SessionTitleService`.
- [`packages/examples/agent-spine-demo/tests/agent-core.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/examples/agent-spine-demo/tests/agent-core.spec.ts) — A test under the owning area exercises or imports `dsh-agent-spine-demo`.
- [`packages/session/session-title/tests/service-contracts.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/session/session-title/tests/service-contracts.spec.ts) — A test under the owning area exercises or imports `SessionTitleService`.
- [`apps/web/tests/schedule-after.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/schedule-after.e2e.ts) — Contains the exact code literal `session/title` named by the note.
- Source verification intent: TUI tests pin restored and live session/title consumption, terminal-safe title rendering, the configured fallback, and the absence of a TUI-owned model path. The keyless PTY smoke boots the real composition, accepts a logged provider title, and observes the resulting terminal title. The log-backed title decision owns provider, persistence, resume, fork, cancellation, and stale-completion coverage.

## How to read the implementation

1. Start with [`packages/llm/llm/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/src/index.ts) because it has the strongest evidence link to the note.
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

- Tags: `class/simplification`, `concern/boundary`, `concern/cancellation-timeout`, `concern/discovery-routing`, `concern/evidence`, `concern/ownership`, `concern/performance`, `concern/recovery`, `domain/build-release`, `domain/configuration`, `domain/extensions`, `domain/llm`, `domain/session-state`, `domain/shell-terminal`, `domain/storage`, `domain/testing`, `domain/ui-interaction`, `lifecycle/archived`, `mechanism/adapter`, `mechanism/event-sourcing`, `mechanism/generation`, `mechanism/policy`
- Aliases: `llm`, `foldSessionTitle`, `SessionTitleService`, `autoTitle`, `runtime.terminal.setTitle`, `setTitle`, `<session title> --- <configured title>`, `session/title`, `examples/tui-agent/cordis.yml`, `@deepseek-ai/dsh-session-title-first-message-llm`, `dsh-agent-spine-demo`, `<title> --- <product>`, `TUI titles come from the session-title service`, `simplification`
- Regex: `(?i)(foldSessionTitle|SessionTitleService|autoTitle|runtime\.terminal\.setTitle|setTitle|<session[- ]title>[- ]\-\-\-[- ]<configured[- ]title>|session/title|examples/tui\-agent/cordis\.yml)`

```bash
rg -n --pcre2 "(?i)(foldSessionTitle|SessionTitleService|autoTitle|runtime\\.terminal\\.setTitle|setTitle|<session[- ]title>[- ]\\-\\-\\-[- ]<configured[- ]title>|session/title|examples/tui\\-agent/cordis\\.yml)" source-deepseek-harness
rg -l --fixed-strings "class/simplification" 03-agent-context
```

## Connected notes

- **`shares-code-with`** — [0572. Auto-title on by default, re-derived on resume](0572-auto-title-on-by-default-re-derived-on-resume.md): Shares source implementation: `packages/llm/llm`, `packages/llm/llm/README.md`.
- **`shares-code-with`** — [0056. Adapter-owned reasoning effort capabilities](0056-adapter-owned-reasoning-effort-capabilities.md): Shares source implementation: `packages/llm/llm`, `packages/llm/llm/README.md`.
- **`shares-code-with`** — [0623. TUI model-context resolution defers on the adapter-registration race](0623-tui-model-context-resolution-defers-on-the-adapter-registration-race.md): Shares source implementation: `packages/llm/llm`, `packages/llm/llm/README.md`.
- **`shares-code-with`** — [0181. Per-provider request retry policies](0181-per-provider-request-retry-policies.md): Shares source implementation: `packages/llm/llm`, `packages/llm/llm/README.md`.
- **`shares-code-with`** — [0650. Drop the unconsumed `llm/adapter-change` event](0650-drop-the-unconsumed-llm-adapter-change-event.md): Shares source implementation: `packages/llm/llm`, `packages/llm/llm/README.md`.
- **`shares-code-with`** — [0567. Dedicated full-screen TUI front door](0567-dedicated-full-screen-tui-front-door.md): Shares source implementation: `packages/llm/llm`, `packages/llm/llm/src/index.ts`.
- **`shares-code-with`** — [0001. Provider-neutral content-block vocabulary owned by dsh-llm](0001-provider-neutral-content-block-vocabulary-owned-by-dsh-llm.md): Shares source implementation: `packages/llm/llm`, `packages/llm/llm/README.md`.
- **`shares-code-with`** — [0571. Auto-titled terminal from the first message](0571-auto-titled-terminal-from-the-first-message.md): Shares source implementation: `packages/llm/llm`, `packages/llm/llm/src/index.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0669-tui-titles-come-from-the-session-title-service.md`.
