---
id: "dsh-note-0102"
title: "Reusable Session preparation before publication"
status: "implemented"
class: "architecture"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/architecture/2026-08-05-session-preparation.md"
implementation_evidence: "medium"
target_anchor: "Rust provider-adapter boundary"
tags:
  - "class/architecture"
  - "concern/boundary"
  - "concern/cancellation-timeout"
  - "concern/compatibility"
  - "concern/concurrency"
  - "concern/evidence"
  - "concern/lifecycle"
  - "concern/ownership"
  - "concern/performance"
  - "concern/recovery"
  - "concern/schema-types"
  - "domain/agent-loop"
  - "domain/build-release"
  - "domain/llm"
  - "domain/multi-agent"
  - "domain/protocols"
  - "domain/session-state"
  - "domain/storage"
  - "domain/testing"
  - "lifecycle/implemented"
  - "mechanism/generation"
  - "mechanism/policy"
aliases:
  - "SessionPreparation"
  - "inspect"
  - "SessionInspection"
  - "SessionStore.prepare"
  - "SessionPersistence.prepare"
  - "preparation.session"
  - "ctx.agents.resume"
  - "readFrom"
  - "persistence.spec.ts"
  - "preparations.spec.ts"
  - "Reusable Session preparation before publication"
  - "architecture"
  - "boundary"
  - "cancellation timeout"
search_regex: "(?i)(SessionPreparation|inspect|SessionInspection|SessionStore\\.prepare|SessionPersistence\\.prepare|preparation\\.session|ctx\\.agents\\.resume|readFrom)"
---

# 0102. Reusable Session preparation before publication — implementation context

## Open this when

Cold history inspection and Agent resume independently materialized the same persisted session log. For a large compressed log, each operation repeated the full read, decompression, parse, validation, freezing, and Session construction. Pagination could therefore pay the cold-read cost again, while making a history query activate an Agent would couple a read lifecycle to a live Agent with no natural retirement point. Fresh creation and persisted resume also reached the same publication boundary through different construction flows.

## Source decision

SessionPreparation owns one exact unpublished Session until publication or rollback. It is a Session lifecycle object, not an Agent lifecycle or activation object. Fresh creation wraps the result of SessionStore.prepare(); persisted resume obtains a preparation from SessionPersistence.prepare(). The Agent loop consumes both forms through one setup-and-publication pipeline: it acquires the preparation, builds the private Agent context around preparation.session, awaits optional setup, publishes that exact Session and Agent, and disposes the preparation on every exit.

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/architecture/2026-08-05-session-preparation.md](../02-notes/implemented/architecture/2026-08-05-session-preparation.md)
- Pinned source: [.agents/notes/implemented/architecture/2026-08-05-session-preparation.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/architecture/2026-08-05-session-preparation.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`packages/core/session/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/index.ts) | package entry point | Core file in the package named by the note: `packages/core/session`. | `named-package-member` |
| [`packages/core/session/src/types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/types.ts) | public types and contract | Core file in the package named by the note: `packages/core/session`. | `named-package-member` |
| [`packages/core/session/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/core/session`. | `named-package-member` |
| [`packages/core/session/src/preparation.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/preparation.ts) | runtime implementation | Core file in the package named by the note: `packages/core/session`. Defines `SessionPreparation`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/core/session`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/packages/core/session) | package or module directory | The note names this package or capability. | `named-package` |
| [`packages/session/session-persistence/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/session/session-persistence/src/index.ts) | package entry point | Defines `SessionInspection`, a construct named by the note. | `symbol-definition` |
| [`packages/extensions/cordis-client-runner/src/client/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/extensions/cordis-client-runner/src/client/index.ts) | package entry point | Defines `inspect`, a construct named by the note. | `symbol-definition` |
| [`packages/core/session/README.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/README.md) | package contract and examples | Core file in the package named by the note: `packages/core/session`. | `named-package-member` |
| [`packages/core/session/package.json`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/package.json) | composition and configuration | Core file in the package named by the note: `packages/core/session`. | `named-package-member` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `SessionPreparation` | `class` | [`packages/core/session/src/preparation.ts:20`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/preparation.ts#L20) | `export class SessionPreparation implements Disposable {` |
| `inspect` | `const` | [`packages/extensions/cordis-client-runner/src/client/index.ts:189`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/extensions/cordis-client-runner/src/client/index.ts#L189) | `const inspect = new ClientCordisInspectRegistry({` |
| `SessionInspection` | `interface` | [`packages/session/session-persistence/src/index.ts:26`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/session/session-persistence/src/index.ts#L26) | `export interface SessionInspection {` |

### Tests and executable evidence

- [`packages/core/session/tests/scoped.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/tests/scoped.spec.ts) — A test under the owning area exercises or imports `prepare`.
- [`packages/core/session/tests/session.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/tests/session.spec.ts) — A test under the owning area exercises or imports `prepare`.
- Source verification intent: The shared persistence contract pins non-mutating balanced cold inspection and later repair. persistence.spec.ts and preparations.spec.ts pin same-id in-flight sharing, exact Session reuse across inspect and prepare, revision-triggered refresh before history and resume, single repair commit, exclusive reservation, release after failed setup, ready-entry LRU eviction, append rejection during reservation, and publication of only the reserved Session. Backend tests pin that full and lightweight reads use the same revision identity.

## How to read the implementation

1. Start with [`packages/core/session/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/index.ts) because it has the strongest evidence link to the note.
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

- Tags: `class/architecture`, `concern/boundary`, `concern/cancellation-timeout`, `concern/compatibility`, `concern/concurrency`, `concern/evidence`, `concern/lifecycle`, `concern/ownership`, `concern/performance`, `concern/recovery`, `concern/schema-types`, `domain/agent-loop`, `domain/build-release`, `domain/llm`, `domain/multi-agent`, `domain/protocols`, `domain/session-state`, `domain/storage`, `domain/testing`, `lifecycle/implemented`, `mechanism/generation`, `mechanism/policy`
- Aliases: `SessionPreparation`, `inspect`, `SessionInspection`, `SessionStore.prepare`, `SessionPersistence.prepare`, `preparation.session`, `ctx.agents.resume`, `readFrom`, `persistence.spec.ts`, `preparations.spec.ts`, `Reusable Session preparation before publication`, `architecture`, `boundary`, `cancellation timeout`
- Regex: `(?i)(SessionPreparation|inspect|SessionInspection|SessionStore\.prepare|SessionPersistence\.prepare|preparation\.session|ctx\.agents\.resume|readFrom)`

```bash
rg -n --pcre2 "(?i)(SessionPreparation|inspect|SessionInspection|SessionStore\\.prepare|SessionPersistence\\.prepare|preparation\\.session|ctx\\.agents\\.resume|readFrom)" source-deepseek-harness
rg -l --fixed-strings "class/architecture" 03-agent-context
```

## Connected notes

- **`source-link`** — [0009. Session persistence as an abstract service over the existing `SessionEvent`](0009-session-persistence-as-an-abstract-service-over-the-existing-sessioneven.md): The source note links to this decision directly.
- **`source-link`** — [0011. Agent lifecycle and ownership contracts](0011-agent-lifecycle-and-ownership-contracts.md): The source note links to this decision directly.
- **`source-link`** — [0013. Shared persistence write coordinator](0013-shared-persistence-write-coordinator.md): The source note links to this decision directly.
- **`source-link`** — [0200. Continuable subagents](0200-continuable-subagents.md): The source note links to this decision directly.
- **`shares-code-with`** — [0311. Load sessions persisted before message identity](0311-load-sessions-persisted-before-message-identity.md): Shares source implementation: `packages/core/session`, `packages/core/session/README.md`.
- **`shares-code-with`** — [0466. Remove synthetic turns for log-only events](0466-remove-synthetic-turns-for-log-only-events.md): Shares source implementation: `packages/core/session`, `packages/core/session/README.md`.
- **`shares-code-with`** — [0447. Fold trace-only session facts into load-bearing events](0447-fold-trace-only-session-facts-into-load-bearing-events.md): Shares source implementation: `packages/core/session`, `packages/core/session/README.md`.
- **`shares-code-with`** — [0171. Default local instruction overlay](0171-default-local-instruction-overlay.md): Shares source implementation: `packages/core/session`, `packages/core/session/README.md`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0102-reusable-session-preparation-before-publication.md`.
