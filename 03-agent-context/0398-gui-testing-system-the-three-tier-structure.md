---
id: "dsh-note-0398"
title: "GUI testing system --- the three-tier structure"
status: "implemented"
class: "process"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/process/2026-07-20-gui-testing-system.md"
implementation_evidence: "high"
target_anchor: "repository tests and engineering policy"
tags:
  - "class/process"
  - "concern/boundary"
  - "concern/concurrency"
  - "concern/discovery-routing"
  - "concern/evidence"
  - "concern/human-control"
  - "concern/lifecycle"
  - "concern/ownership"
  - "concern/schema-types"
  - "domain/build-release"
  - "domain/configuration"
  - "domain/extensions"
  - "domain/llm"
  - "domain/protocols"
  - "domain/session-state"
  - "domain/storage"
  - "domain/testing"
  - "domain/ui-interaction"
  - "domain/web-retrieval"
  - "lifecycle/implemented"
  - "mechanism/adapter"
  - "mechanism/event-sourcing"
  - "mechanism/policy"
  - "mechanism/projection"
  - "mechanism/registry"
  - "mechanism/streaming"
aliases:
  - "ConnectionController"
  - "SessionManager"
  - "title"
  - "refresh"
  - "AbstractApiClient"
  - "InProcessApiClient"
  - "toFetchHandler"
  - "runtime"
  - "packages/client/runtime/tests/"
  - "packages/client/connection/tests/"
  - "web-ui"
  - "packages/client/*/tests/"
  - "createXXXStore().create"
  - "createXXXStore"
search_regex: "(?i)(ConnectionController|SessionManager|title|refresh|AbstractApiClient|InProcessApiClient|toFetchHandler|runtime)"
---

# 0398. GUI testing system --- the three-tier structure — implementation context

## Open this when

The GUI stack spans multiple application shapes, and within one shape multiple runtime environments (the Node host, the data protocol layer, the browser object layer, React/DOM); a single-lane test suite cannot give a meaningful signal. Every link needs effective tests of its own, plus the base capability for full-chain testing.

## Source decision

Cut along the architecture's natural test hooks into three tiers, bottom-up: Inter-tier discipline: each tier tests its own layer, upper tiers never re-test lower ones --- an app semantic snapshot pins only user-visible projection across the assembled plugin boundary, while Playwright smoke proves browser and carrier liveness; wire semantics belong to tier 1 and data semantics to tier 2. Pure-function layers (lineage/partial/notifier/transcript-adapter) are tested directly with zero fakes in the same package's tests/ alongside tier 2.

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/process/2026-07-20-gui-testing-system.md](../02-notes/implemented/process/2026-07-20-gui-testing-system.md)
- Pinned source: [.agents/notes/implemented/process/2026-07-20-gui-testing-system.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/process/2026-07-20-gui-testing-system.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`docs/testing.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/testing.md) | package contract and examples | The source note names this file directly. | `named-file` |
| [`packages/core/session/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/index.ts) | package entry point | Core file in the package named by the note: `packages/core/session`. | `named-package-member` |
| [`packages/core/session/src/types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/types.ts) | public types and contract | Core file in the package named by the note: `packages/core/session`. | `named-package-member` |
| [`packages/client/runtime/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/runtime/src/index.ts) | package entry point | Core file in the package named by the note: `packages/client/runtime`. | `named-package-member` |
| [`packages/client/ui-slots/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-slots/src/index.ts) | package entry point | Core file in the package named by the note: `packages/client/ui-slots`. | `named-package-member` |
| [`packages/core/session/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/core/session`. | `named-package-member` |
| [`packages/client/runtime/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/runtime/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/client/runtime`. | `named-package-member` |
| [`packages/client/ui-slots/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-slots/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/client/ui-slots`. | `named-package-member` |
| [`packages/test-support/llm-replay/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/test-support/llm-replay/src/index.ts) | package entry point | Core file in the package named by the note: `packages/test-support/llm-replay`. | `named-package-member` |
| [`packages/test-support/llm-replay/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/test-support/llm-replay/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/test-support/llm-replay`. | `named-package-member` |
| [`packages/client/runtime/src/client/sessions/manager.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/runtime/src/client/sessions/manager.ts) | runtime implementation | Entry point or contract under the directory named by the note: `packages/client`. Core file in the package named by the note: `packages/client/runtime`. | `named-directory-member, named-package-member, symbol-definition` |
| [`packages/client/runtime/src/client/sessions/session.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/runtime/src/client/sessions/session.ts) | runtime implementation | Core file in the package named by the note: `packages/client/runtime`. | `named-package-member` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `ConnectionController` | `class` | [`packages/client/connection/src/client/connection.ts:61`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/connection/src/client/connection.ts#L61) | `export class ConnectionController {` |
| `SessionManager` | `class` | [`packages/client/runtime/src/client/sessions/manager.ts:106`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/runtime/src/client/sessions/manager.ts#L106) | `export class SessionManager {` |
| `title` | `const` | [`packages/client/runtime/src/client/sessions/manager.ts:1025`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/runtime/src/client/sessions/manager.ts#L1025) | `const title = projectionStore?.get('title')` |
| `refresh` | `const` | [`packages/client/ui-agent-preset/src/client/index.ts:77`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-agent-preset/src/client/index.ts#L77) | `const refresh = (): void => {` |
| `AbstractApiClient` | `class` | [`packages/host/apiproxy/src/fetch/client.ts:244`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/src/fetch/client.ts#L244) | `export abstract class AbstractApiClient implements IApiClient {` |
| `InProcessApiClient` | `class` | [`packages/host/apiproxy/src/fetch/client.ts:520`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/src/fetch/client.ts#L520) | `export class InProcessApiClient extends AbstractApiClient {` |
| `toFetchHandler` | `function` | [`packages/host/apiproxy/src/fetch/handler.ts:243`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/src/fetch/handler.ts#L243) | `export function toFetchHandler(api: ApiProxy): { fetch: typeof fetch } {` |
| `runtime` | `const` | [`vendor/cordis/src/registry.ts:260`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/cordis/src/registry.ts#L260) | `const runtime = key && this._internal.get(key)` |

### Tests and executable evidence

- [`packages/host/apiproxy/tests/client-handler.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/tests/client-handler.spec.ts) — The source note names this file directly. A test under the owning area exercises or imports `toFetchHandler`.
- [`apps/web/tests/README.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/README.md) — Entry point or contract under the directory named by the note: `apps/web/tests`.
- [`packages/host/apiproxy/tests/fetch-carrier.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/tests/fetch-carrier.spec.ts) — A test under the owning area exercises or imports `AbstractApiClient`. A test under the owning area exercises or imports `toFetchHandler`.
- [`packages/host/apiproxy/tests/session-export.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/tests/session-export.spec.ts) — A test under the owning area exercises or imports `toFetchHandler`.
- [`packages/client/runtime/tests/manager.client.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/runtime/tests/manager.client.spec.ts) — A test under the owning area exercises or imports `SessionManager`.
- [`packages/client/runtime/tests/queue-store.client.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/runtime/tests/queue-store.client.spec.ts) — A test under the owning area exercises or imports `SessionManager`.
- [`packages/client/connection/tests/connection.client.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/connection/tests/connection.client.spec.ts) — A test under the owning area exercises or imports `ConnectionController`.
- [`packages/client/runtime/tests/projection-store.client.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/runtime/tests/projection-store.client.spec.ts) — A test under the owning area exercises or imports `SessionManager`.

## How to read the implementation

1. Start with [`docs/testing.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/testing.md) because it has the strongest evidence link to the note.
2. Read public types and entry points before implementation details. Write down the contract in plain language.
3. Follow the named constructs into their callers and tests. Confirm which component can actually enforce the rule.
4. Reconstruct one failure path: cancellation, crash, retry, stale state, partial write, or incompatible provider input—whichever the note is protecting against.
5. Only then map the contract into the current Rust repository. Target paths must come from that repository, never from guesswork in this wiki.

## Rust harness port

**Likely integration point:** repository tests and engineering policy.

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
- **`rust/trait-seam`** — Use a small trait at the capability boundary; keep policy and model-facing schemas outside the backend implementation.

### Smallest safe implementation slice

1. Express the semantic contract as Rust types and one owning boundary.
2. Add the smallest adapter or service needed by the existing loop; do not add a new model-facing tool unless the four-tool surface cannot express the job.
3. Record durable evidence before deriving summaries or UI state.
4. Add one happy-path test, one interruption test, and one replay/restart test where state survives a turn.
5. Keep the patch reversible until the source note's strongest failure case is proven in the Rust harness.

## Search handles

- Tags: `class/process`, `concern/boundary`, `concern/concurrency`, `concern/discovery-routing`, `concern/evidence`, `concern/human-control`, `concern/lifecycle`, `concern/ownership`, `concern/schema-types`, `domain/build-release`, `domain/configuration`, `domain/extensions`, `domain/llm`, `domain/protocols`, `domain/session-state`, `domain/storage`, `domain/testing`, `domain/ui-interaction`, `domain/web-retrieval`, `lifecycle/implemented`, `mechanism/adapter`, `mechanism/event-sourcing`, `mechanism/policy`, `mechanism/projection`, `mechanism/registry`, `mechanism/streaming`
- Aliases: `ConnectionController`, `SessionManager`, `title`, `refresh`, `AbstractApiClient`, `InProcessApiClient`, `toFetchHandler`, `runtime`, `packages/client/runtime/tests/`, `packages/client/connection/tests/`, `web-ui`, `packages/client/*/tests/`, `createXXXStore().create`, `createXXXStore`
- Regex: `(?i)(ConnectionController|SessionManager|title|refresh|AbstractApiClient|InProcessApiClient|toFetchHandler|runtime)`

```bash
rg -n --pcre2 "(?i)(ConnectionController|SessionManager|title|refresh|AbstractApiClient|InProcessApiClient|toFetchHandler|runtime)" source-deepseek-harness
rg -l --fixed-strings "class/process" 03-agent-context
```

## Connected notes

- **`source-link`** — [0052. The slot system standard --- single register, four props shares, and the framework store seat](0052-the-slot-system-standard-single-register-four-props-shares-and-the-frame.md): The source note links to this decision directly.
- **`source-link`** — [0500. Keyless browser e2e lane for the web GUI](0500-keyless-browser-e2e-lane-for-the-web-gui.md): The source note links to this decision directly.
- **`source-link`** — [0503. Required CI gate for web browser expected outputs](0503-required-ci-gate-for-web-browser-expected-outputs.md): The source note links to this decision directly.
- **`shares-code-with`** — [0495. ACP snapshot tests --- record-once / replay-deterministic](0495-acp-snapshot-tests-record-once-replay-deterministic.md): Shares source implementation: `packages/core/session/src/index.ts`, `packages/core/session/src/invariant.ts`.
- **`shares-code-with`** — [0198. Web subagent catalog and human continuation](0198-web-subagent-catalog-and-human-continuation.md): Shares source implementation: `packages/core/session/src/index.ts`, `packages/core/session/src/invariant.ts`.
- **`shares-code-with`** — [0497. Persist the seed boundary so fork-child replay routes correctly](0497-persist-the-seed-boundary-so-fork-child-replay-routes-correctly.md): Shares source implementation: `packages/core/session/src/index.ts`, `packages/core/session/src/invariant.ts`.
- **`shares-code-with`** — [0518. Recallable compaction --- index checkpoints, a state checkpoint, and in-session history recall](0518-recallable-compaction-index-checkpoints-a-state-checkpoint-and-in-sessio.md): Shares source implementation: `packages/core/session/src/index.ts`, `packages/core/session/src/invariant.ts`.
- **`shares-code-with`** — [0265. Settlement delivery belongs to the continuation manager](0265-settlement-delivery-belongs-to-the-continuation-manager.md): Shares source implementation: `packages/core/session/src/index.ts`, `packages/core/session/src/invariant.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0398-gui-testing-system-the-three-tier-structure.md`.
