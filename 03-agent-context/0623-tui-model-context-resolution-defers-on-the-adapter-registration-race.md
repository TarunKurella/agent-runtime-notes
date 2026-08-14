---
id: "dsh-note-0623"
title: "TUI model-context resolution defers on the adapter-registration race"
status: "archived"
class: "bug-fix"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/archived/bug-fix/2026-07-30-tui-adapter-registration-race.md"
implementation_evidence: "medium"
target_anchor: "Rust provider-adapter boundary"
tags:
  - "class/bug-fix"
  - "concern/boundary"
  - "concern/concurrency"
  - "concern/discovery-routing"
  - "concern/evidence"
  - "concern/lifecycle"
  - "concern/ownership"
  - "concern/performance"
  - "concern/recovery"
  - "domain/configuration"
  - "domain/context"
  - "domain/extensions"
  - "domain/llm"
  - "domain/session-state"
  - "domain/ui-interaction"
  - "lifecycle/archived"
  - "mechanism/adapter"
  - "mechanism/registry"
aliases:
  - "detach"
  - "inject"
  - "llm"
  - "dsh-llm-pi-ai"
  - "resolveModelInfo"
  - "NO_ADAPTER"
  - "llm/adapters-updated"
  - "LlmService"
  - "prepareCall"
  - "detachListeners"
  - "TUI model-context resolution defers on the adapter-registration race"
  - "bug fix"
  - "boundary"
  - "concurrency"
search_regex: "(?i)(detach|inject|dsh\\-llm\\-pi\\-ai|resolveModelInfo|NO_ADAPTER|llm/adapters\\-updated|LlmService|prepareCall)"
---

# 0623. TUI model-context resolution defers on the adapter-registration race — implementation context

## Open this when

Cordis activates plugins by service availability, not configuration order, so the TUI (whose inject requires only the llm service) can mount before a configured adapter plugin such as dsh-llm-pi-ai finishes registering its provider routes. The TUI's model controller resolves the selected model's context window immediately on mount; when the agent's route pointed at a not-yet-registered provider, resolveModelInfo rejected with NO_ADAPTER and every fresh session printed Could not resolve model context: no adapter registered for provider "…" --- a spurious error for a fully working configuration (the adapter.

## Source decision

The TUI model controller treats a NO_ADAPTER rejection of its context-window resolution as a transient state rather than an error: it parks the resolution silently and re-resolves on the next llm/adapters-updated commit --- the payload-free registry notification LlmService already fires at every route commit point. A commit that still lacks the route parks the wait again, so unrelated topology changes stay silent. Any target change re-enters the resolution and clears the pending wait, so the deferred state can never go stale against the current selection; every other resolution error still prints the notice.

## Decision status

Historical design. Use it to understand an older pressure or failure; locate the replacing decision before implementing anything.

- Raw note: [archived/bug-fix/2026-07-30-tui-adapter-registration-race.md](../02-notes/archived/bug-fix/2026-07-30-tui-adapter-registration-race.md)
- Pinned source: [.agents/notes/archived/bug-fix/2026-07-30-tui-adapter-registration-race.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/archived/bug-fix/2026-07-30-tui-adapter-registration-race.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`packages/llm/llm/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/src/index.ts) | package entry point | Core file in the package named by the note: `packages/llm/llm`. | `named-package-member` |
| [`packages/llm/llm/src/types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/src/types.ts) | public types and contract | Core file in the package named by the note: `packages/llm/llm`. | `named-package-member` |
| [`packages/llm/llm/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/llm/llm`. Defines `inject`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/llm/llm-pi-ai/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm-pi-ai/src/index.ts) | package entry point | Core file in the package named by the note: `packages/llm/llm-pi-ai`. Defines `inject`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/llm/llm-pi-ai/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm-pi-ai/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/llm/llm-pi-ai`. Defines `inject`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/llm/llm`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm) | package or module directory | The note names this package or capability. | `named-package` |
| [`packages/llm/llm-pi-ai`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm-pi-ai) | package or module directory | The note names this package or capability. | `named-package` |
| [`packages/core/agent/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent/src/index.ts) | package entry point | Defines `detach`, a construct named by the note. | `symbol-definition` |
| [`packages/llm/llm/README.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/README.md) | package contract and examples | Core file in the package named by the note: `packages/llm/llm`. | `named-package-member` |
| [`packages/llm/llm/package.json`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/package.json) | composition and configuration | Core file in the package named by the note: `packages/llm/llm`. | `named-package-member` |
| [`packages/llm/llm-pi-ai/README.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm-pi-ai/README.md) | package contract and examples | Core file in the package named by the note: `packages/llm/llm-pi-ai`. | `named-package-member` |
| [`packages/llm/llm-pi-ai/package.json`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm-pi-ai/package.json) | composition and configuration | Core file in the package named by the note: `packages/llm/llm-pi-ai`. | `named-package-member` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `detach` | `const` | [`packages/core/agent/src/index.ts:494`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent/src/index.ts#L494) | `const detach = (): void => {` |
| `inject` | `const` | [`packages/llm/llm-pi-ai/src/index.ts:85`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm-pi-ai/src/index.ts#L85) | `export const inject = ['llm']` |
| `inject` | `const` | [`packages/llm/llm-pi-ai/src/invariant.ts:15`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm-pi-ai/src/invariant.ts#L15) | `export const inject = ['invariants']` |
| `inject` | `const` | [`packages/llm/llm/src/invariant.ts:12`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/src/invariant.ts#L12) | `export const inject = ['invariants']` |
| `llm` | `const` | [`packages/llm/llm/src/invariant.ts:92`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/src/invariant.ts#L92) | `const llm = ctx.get('llm')` |

### Tests and executable evidence

- [`packages/llm/llm/tests/service.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/tests/service.spec.ts) — A test under the owning area exercises or imports `resolveModelInfo`. A test under the owning area exercises or imports `NO_ADAPTER`.
- [`packages/core/agent/tests/agent.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent/tests/agent.spec.ts) — A test under the owning area exercises or imports `detach`.
- [`packages/llm/llm-pi-ai/tests/adapter.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm-pi-ai/tests/adapter.e2e.ts) — A test under the owning area exercises or imports `dsh-llm-pi-ai`.
- [`packages/llm/llm-pi-ai/tests/catalog.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm-pi-ai/tests/catalog.spec.ts) — A test under the owning area exercises or imports `dsh-llm-pi-ai`. A test under the owning area exercises or imports `resolveModelInfo`.
- [`packages/llm/llm-pi-ai/tests/adapter.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm-pi-ai/tests/adapter.spec.ts) — A test under the owning area exercises or imports `dsh-llm-pi-ai`. A test under the owning area exercises or imports `resolveModelInfo`.
- [`packages/llm/llm-pi-ai/tests/discovery.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm-pi-ai/tests/discovery.spec.ts) — A test under the owning area exercises or imports `dsh-llm-pi-ai`.
- [`packages/llm/llm-pi-ai/tests/provider-apis.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm-pi-ai/tests/provider-apis.e2e.ts) — A test under the owning area exercises or imports `dsh-llm-pi-ai`.
- [`packages/llm/llm-pi-ai/tests/dynamic-config.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm-pi-ai/tests/dynamic-config.spec.ts) — A test under the owning area exercises or imports `dsh-llm-pi-ai`. A test under the owning area exercises or imports `NO_ADAPTER`.

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
- **`rust/context-evidence`** — Compaction may shrink presentation, but it must not erase the evidence ledger, file activity, or durable session facts.
- **`rust/four-tool-surface`** — Preserve the four model tools: read, write, patch, and exec. Reuse internal capabilities without automatically exposing new model-facing tools.
- **`rust/provider-adapter`** — Keep provider wire quirks inside adapters; the loop and durable event vocabulary remain provider-neutral.
- **`rust/quickjs-boundary`** — QuickJS holds orchestration variables; Rust/Tokio owns async execution and exposes only explicitly granted capabilities.
- **`rust/trait-seam`** — Use a small trait at the capability boundary; keep policy and model-facing schemas outside the backend implementation.
- **Status gate** — Historical design. Use it to understand an older pressure or failure; locate the replacing decision before implementing anything.

### Smallest safe implementation slice

1. Express the semantic contract as Rust types and one owning boundary.
2. Add the smallest adapter or service needed by the existing loop; do not add a new model-facing tool unless the four-tool surface cannot express the job.
3. Record durable evidence before deriving summaries or UI state.
4. Add one happy-path test, one interruption test, and one replay/restart test where state survives a turn.
5. Keep the patch reversible until the source note's strongest failure case is proven in the Rust harness.

## Search handles

- Tags: `class/bug-fix`, `concern/boundary`, `concern/concurrency`, `concern/discovery-routing`, `concern/evidence`, `concern/lifecycle`, `concern/ownership`, `concern/performance`, `concern/recovery`, `domain/configuration`, `domain/context`, `domain/extensions`, `domain/llm`, `domain/session-state`, `domain/ui-interaction`, `lifecycle/archived`, `mechanism/adapter`, `mechanism/registry`
- Aliases: `detach`, `inject`, `llm`, `dsh-llm-pi-ai`, `resolveModelInfo`, `NO_ADAPTER`, `llm/adapters-updated`, `LlmService`, `prepareCall`, `detachListeners`, `TUI model-context resolution defers on the adapter-registration race`, `bug fix`, `boundary`, `concurrency`
- Regex: `(?i)(detach|inject|dsh\-llm\-pi\-ai|resolveModelInfo|NO_ADAPTER|llm/adapters\-updated|LlmService|prepareCall)`

```bash
rg -n --pcre2 "(?i)(detach|inject|dsh\\-llm\\-pi\\-ai|resolveModelInfo|NO_ADAPTER|llm/adapters\\-updated|LlmService|prepareCall)" source-deepseek-harness
rg -l --fixed-strings "class/bug-fix" 03-agent-context
```

## Connected notes

- **`shares-code-with`** — [0669. TUI titles come from the session-title service](0669-tui-titles-come-from-the-session-title-service.md): Shares source implementation: `packages/llm/llm`, `packages/llm/llm/README.md`.
- **`shares-code-with`** — [0056. Adapter-owned reasoning effort capabilities](0056-adapter-owned-reasoning-effort-capabilities.md): Shares source implementation: `packages/llm/llm`, `packages/llm/llm/README.md`.
- **`shares-code-with`** — [0001. Provider-neutral content-block vocabulary owned by dsh-llm](0001-provider-neutral-content-block-vocabulary-owned-by-dsh-llm.md): Shares source implementation: `packages/llm/llm`, `packages/llm/llm/README.md`.
- **`shares-code-with`** — [0650. Drop the unconsumed `llm/adapter-change` event](0650-drop-the-unconsumed-llm-adapter-change-event.md): Shares source implementation: `packages/llm/llm`, `packages/llm/llm/README.md`.
- **`shares-code-with`** — [0572. Auto-title on by default, re-derived on resume](0572-auto-title-on-by-default-re-derived-on-resume.md): Shares source implementation: `packages/llm/llm`, `packages/llm/llm/README.md`.
- **`shares-code-with`** — [0181. Per-provider request retry policies](0181-per-provider-request-retry-policies.md): Shares source implementation: `packages/llm/llm`, `packages/llm/llm/README.md`.
- **`shares-code-with`** — [0567. Dedicated full-screen TUI front door](0567-dedicated-full-screen-tui-front-door.md): Shares source implementation: `packages/llm/llm`, `packages/llm/llm/src/index.ts`.
- **`shares-code-with`** — [0037. Provider-routed LLM adapters and a generic pi-ai backend](0037-provider-routed-llm-adapters-and-a-generic-pi-ai-backend.md): Shares source implementation: `packages/llm/llm-pi-ai/src/index.ts`, `packages/llm/llm/src/index.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0623-tui-model-context-resolution-defers-on-the-adapter-registration-race.md`.
