---
id: "dsh-note-0172"
title: "Log-backed session titles"
status: "implemented"
class: "feature"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/feature/2026-07-21-log-backed-session-titles.md"
implementation_evidence: "medium"
target_anchor: "Rust provider-adapter boundary"
tags:
  - "class/feature"
  - "concern/boundary"
  - "concern/cancellation-timeout"
  - "concern/concurrency"
  - "concern/discovery-routing"
  - "concern/human-control"
  - "concern/lifecycle"
  - "concern/ownership"
  - "concern/performance"
  - "concern/recovery"
  - "concern/schema-types"
  - "concern/simplification"
  - "domain/build-release"
  - "domain/context"
  - "domain/llm"
  - "domain/protocols"
  - "domain/session-state"
  - "domain/shell-terminal"
  - "domain/storage"
  - "domain/testing"
  - "domain/ui-interaction"
  - "domain/web-retrieval"
  - "lifecycle/implemented"
  - "mechanism/adapter"
  - "mechanism/event-sourcing"
  - "mechanism/generation"
  - "mechanism/policy"
  - "mechanism/projection"
  - "mechanism/registry"
  - "mechanism/streaming"
aliases:
  - "user"
  - "refresh"
  - "sessionTitle"
  - "foldConsumedWork"
  - "surface"
  - "SessionHeader"
  - "list"
  - "provider"
  - "GenerateOptions"
  - "readTitle"
  - "title"
  - "SessionTitleSnapshot"
  - "foldSessionTitle"
  - "model"
search_regex: "(?i)(user|refresh|sessionTitle|foldConsumedWork|surface|SessionHeader|list|provider)"
---

# 0172. Log-backed session titles — implementation context

## Open this when

A session needs a short human-facing title before an editor, terminal, or query consumer can present it usefully. The cheapest implementation can derive one from the first prompt, while higher-quality implementations may call a model over the first prompt or the whole conversation. Those strategies have different latency, cost, routing, and retry behavior, but every consumer needs one durable source of truth. Session identity metadata is immutable, and the event log is the replay and fork boundary.

## Source decision

The session-title capability family owns title state and generation policy. @deepseek-ai/dsh-session-title provides ctx.sessionTitle, a deterministic first-prompt fallback, and a registry for at most one optional asynchronous provider. @deepseek-ai/dsh-session-title-llm owns the common auxiliary-model request policy; separate first-prompt and all-prompts plugins choose input cadence. The shared agent spine mounts only the fallback service.

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/feature/2026-07-21-log-backed-session-titles.md](../02-notes/implemented/feature/2026-07-21-log-backed-session-titles.md)
- Pinned source: [.agents/notes/implemented/feature/2026-07-21-log-backed-session-titles.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/feature/2026-07-21-log-backed-session-titles.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`packages/session/README.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/session/README.md) | package contract and examples | The source note names this file directly. | `named-file` |
| [`packages/session/session-title/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/session/session-title/src/index.ts) | package entry point | Core file in the package named by the note: `packages/session/session-title`. Defines `foldSessionTitle`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/session/session-title/src/types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/session/session-title/src/types.ts) | public types and contract | Core file in the package named by the note: `packages/session/session-title`. | `named-package-member` |
| [`packages/session/session-title/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/session/session-title/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/session/session-title`. | `named-package-member` |
| [`packages/session/session-title-llm/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/session/session-title-llm/src/index.ts) | package entry point | Core file in the package named by the note: `packages/session/session-title-llm`. Defines `title`, a construct named by the note. | `exact-code-occurrence, named-package-member, symbol-definition` |
| [`packages/session/session-title-llm/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/session/session-title-llm/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/session/session-title-llm`. | `named-package-member` |
| [`packages/session/session-title`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/packages/session/session-title) | package or module directory | The note names this package or capability. | `named-package` |
| [`packages/session/session-title-llm`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/packages/session/session-title-llm) | package or module directory | The note names this package or capability. | `named-package` |
| [`packages/llm/llm/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/src/index.ts) | package entry point | Defines `provider`, a construct named by the note. | `symbol-definition` |
| [`packages/llm/llm/src/types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/src/types.ts) | public types and contract | Defines `GenerateOptions`, a construct named by the note. | `symbol-definition` |
| [`packages/core/session/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/index.ts) | package entry point | Defines `surface`, a construct named by the note. | `symbol-definition` |
| [`packages/core/session/src/types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/types.ts) | public types and contract | Defines `SessionHeader`, a construct named by the note. | `symbol-definition` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `user` | `const` | [`packages/boot/app-boot/src/index.ts:185`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/boot/app-boot/src/index.ts#L185) | `const user = home === resolve(cwd) ? undefined : readEnvLayer(binName, home, warn)` |
| `refresh` | `const` | [`packages/client/ui-agent-preset/src/client/index.ts:77`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-agent-preset/src/client/index.ts#L77) | `const refresh = (): void => {` |
| `sessionTitle` | `function` | [`packages/client/ui-workspace/src/client/tree.ts:129`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-workspace/src/client/tree.ts#L129) | `function sessionTitle(session: SessionSummary): string {` |
| `foldConsumedWork` | `function` | [`packages/core/agent/src/consumed-work.ts:68`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent/src/consumed-work.ts#L68) | `export function foldConsumedWork(events: readonly SessionEvent[]): ConsumedWork {` |
| `surface` | `const` | [`packages/core/session/src/index.ts:727`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/index.ts#L727) | `const surface = this.surface` |
| `SessionHeader` | `interface` | [`packages/core/session/src/types.ts:61`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/types.ts#L61) | `export interface SessionHeader {` |
| `list` | `const` | [`packages/hooks/hook-protocol/src/merge.ts:75`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/hooks/hook-protocol/src/merge.ts#L75) | `const list = reasonsByRank.get(r) ?? []` |
| `provider` | `const` | [`packages/llm/llm/src/index.ts:632`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/src/index.ts#L632) | `const provider = registration.provider.id` |
| `GenerateOptions` | `interface` | [`packages/llm/llm/src/types.ts:320`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/src/types.ts#L320) | `export interface GenerateOptions {` |
| `readTitle` | `function` | [`packages/session-query/tool-session-query/src/workspace-access.ts:155`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/session-query/tool-session-query/src/workspace-access.ts#L155) | `async function readTitle(` |
| `title` | `const` | [`packages/session/session-title-llm/src/index.ts:287`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/session/session-title-llm/src/index.ts#L287) | `const title = normalizeSessionTitle(text, Number.MAX_SAFE_INTEGER)` |
| `SessionTitleSnapshot` | `interface` | [`packages/session/session-title/src/index.ts:71`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/session/session-title/src/index.ts#L71) | `export interface SessionTitleSnapshot extends SessionTitleEventData {` |
| `foldSessionTitle` | `function` | [`packages/session/session-title/src/index.ts:191`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/session/session-title/src/index.ts#L191) | `export function foldSessionTitle(events: readonly SessionEvent[]): SessionTitleSnapshot \| undefined {` |
| `title` | `const` | [`packages/session/session-title/src/index.ts:595`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/session/session-title/src/index.ts#L595) | `const title = normalizeSessionTitle(candidate.title, this.config.maxTitleBytes)` |
| `title` | `const` | [`packages/session/session-title/src/index.ts:745`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/session/session-title/src/index.ts#L745) | `const title = fallbackSessionTitle(first.text, this.config.fallbackMaxWords, this.config.fallbackMaxBytes)` |
| `title` | `const` | [`packages/session/session-title/src/index.ts:761`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/session/session-title/src/index.ts#L761) | `const title = fallbackSessionTitle(` |

### Tests and executable evidence

- [`packages/llm/llm/tests/service.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/tests/service.spec.ts) — A test under the owning area exercises or imports `GenerateOptions`.
- [`packages/llm/llm/tests/topology.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/tests/topology.spec.ts) — A test under the owning area exercises or imports `GenerateOptions`.
- [`packages/llm/llm/tests/invariant.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/tests/invariant.spec.ts) — A test under the owning area exercises or imports `GenerateOptions`.
- [`packages/llm/llm/tests/call-config.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/tests/call-config.spec.ts) — A test under the owning area exercises or imports `GenerateOptions`.
- [`packages/core/agent/tests/consumed-work.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent/tests/consumed-work.spec.ts) — A test under the owning area exercises or imports `foldConsumedWork`.
- [`packages/session/session-title/tests/rename.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/session/session-title/tests/rename.spec.ts) — A test under the owning area exercises or imports `sessionTitle`. A test under the owning area exercises or imports `foldSessionTitle`.
- [`packages/session/session-title/tests/provider.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/session/session-title/tests/provider.spec.ts) — A test under the owning area exercises or imports `sessionTitle`.
- [`packages/session/session-title/tests/projection.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/session/session-title/tests/projection.spec.ts) — A test under the owning area exercises or imports `foldSessionTitle`.

## How to read the implementation

1. Start with [`packages/session/README.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/session/README.md) because it has the strongest evidence link to the note.
2. Read public types and entry points before implementation details. Write down the contract in plain language.
3. Follow the named constructs into their callers and tests. Confirm which component can actually enforce the rule.
4. Reconstruct one failure path: cancellation, crash, retry, stale state, partial write, or incompatible provider input—whichever the note is protecting against.
5. Only then map the contract into the current Rust repository. Target paths must come from that repository, never from guesswork in this wiki.

## Rust harness port

**Likely integration point:** Rust provider-adapter boundary.

Preserve the behavior and ownership rule. Do not mechanically translate Cordis, TypeScript declaration merging, Node streams, or package topology into Rust.

Relevant standing rules:

- **`rust/append-log`** — Keep durable facts append-only and derive views from them. Separate the synchronous fact boundary from asynchronous persistence.
- **`rust/context-evidence`** — Compaction may shrink presentation, but it must not erase the evidence ledger, file activity, or durable session facts.
- **`rust/four-tool-surface`** — Preserve the four model tools: read, write, patch, and exec. Reuse internal capabilities without automatically exposing new model-facing tools.
- **`rust/newtype-ids`** — Represent identity domains with Rust newtypes; do not pass bare strings across session, call, turn, step, and agent boundaries.
- **`rust/process-discipline`** — Treat this as repository or release policy unless it changes a runtime contract. Do not add runtime machinery for a process-only lesson.
- **`rust/provider-adapter`** — Keep provider wire quirks inside adapters; the loop and durable event vocabulary remain provider-neutral.
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

- Tags: `class/feature`, `concern/boundary`, `concern/cancellation-timeout`, `concern/concurrency`, `concern/discovery-routing`, `concern/human-control`, `concern/lifecycle`, `concern/ownership`, `concern/performance`, `concern/recovery`, `concern/schema-types`, `concern/simplification`, `domain/build-release`, `domain/context`, `domain/llm`, `domain/protocols`, `domain/session-state`, `domain/shell-terminal`, `domain/storage`, `domain/testing`, `domain/ui-interaction`, `domain/web-retrieval`, `lifecycle/implemented`, `mechanism/adapter`, `mechanism/event-sourcing`, `mechanism/generation`, `mechanism/policy`, `mechanism/projection`, `mechanism/registry`, `mechanism/streaming`
- Aliases: `user`, `refresh`, `sessionTitle`, `foldConsumedWork`, `surface`, `SessionHeader`, `list`, `provider`, `GenerateOptions`, `readTitle`, `title`, `SessionTitleSnapshot`, `foldSessionTitle`, `model`
- Regex: `(?i)(user|refresh|sessionTitle|foldConsumedWork|surface|SessionHeader|list|provider)`

```bash
rg -n --pcre2 "(?i)(user|refresh|sessionTitle|foldConsumedWork|surface|SessionHeader|list|provider)" source-deepseek-harness
rg -l --fixed-strings "class/feature" 03-agent-context
```

## Connected notes

- **`source-link`** — [0466. Remove synthetic turns for log-only events](0466-remove-synthetic-turns-for-log-only-events.md): The source note links to this decision directly.
- **`shares-code-with`** — [0331. Resume selector folds titles only](0331-resume-selector-folds-titles-only.md): Shares source implementation: `packages/session/session-title`, `packages/session/session-title/src/index.ts`.
- **`shares-code-with`** — [0500. Keyless browser e2e lane for the web GUI](0500-keyless-browser-e2e-lane-for-the-web-gui.md): Shares source implementation: `packages/llm/llm/src/index.ts`, `packages/llm/llm/src/types.ts`.
- **`shares-code-with`** — [0133. Compaction as a capability seam (abstract contract + basic backend)](0133-compaction-as-a-capability-seam-abstract-contract-basic-backend.md): Shares source implementation: `packages/llm/llm/src/index.ts`, `packages/llm/llm/src/types.ts`.
- **`shares-code-with`** — [0016. Bounded recovery for transient LLM request failures](0016-bounded-recovery-for-transient-llm-request-failures.md): Shares source implementation: `packages/llm/llm/src/index.ts`, `packages/llm/llm/src/types.ts`.
- **`shares-code-with`** — [0669. TUI titles come from the session-title service](0669-tui-titles-come-from-the-session-title-service.md): Shares source implementation: `packages/llm/llm/src/index.ts`, `packages/llm/llm/src/types.ts`.
- **`shares-code-with`** — [0131. Code Mode --- the model writes TypeScript against the tool registry](0131-code-mode-the-model-writes-typescript-against-the-tool-registry.md): Shares source implementation: `packages/llm/llm/src/index.ts`, `packages/llm/llm/src/types.ts`.
- **`shares-code-with`** — [0037. Provider-routed LLM adapters and a generic pi-ai backend](0037-provider-routed-llm-adapters-and-a-generic-pi-ai-backend.md): Shares source implementation: `packages/llm/llm/src/index.ts`, `packages/llm/llm/src/types.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0172-log-backed-session-titles.md`.
