---
id: "dsh-note-0536"
title: "Fold the persistence interface into dsh-session"
status: "rejected"
class: "simplification"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/rejected/simplification/2026-06-20-fold-session-persistence-interface.md"
implementation_evidence: "medium"
target_anchor: "Rust provider-adapter boundary"
tags:
  - "class/simplification"
  - "concern/boundary"
  - "concern/concurrency"
  - "concern/lifecycle"
  - "concern/ownership"
  - "concern/performance"
  - "concern/recovery"
  - "domain/build-release"
  - "domain/extensions"
  - "domain/llm"
  - "domain/session-state"
  - "domain/shell-terminal"
  - "domain/storage"
  - "domain/testing"
  - "lifecycle/rejected"
  - "mechanism/registry"
aliases:
  - "SessionId"
  - "SessionHeader"
  - "SessionEvent"
  - "SessionPersistence"
  - "dsh-session"
  - "dsh-session-persistence"
  - "session/event"
  - "session/flush"
  - "agent-loop"
  - "@deepseek-ai/dsh-session-persistence"
  - "Fold the persistence interface into dsh-session"
  - "simplification"
  - "boundary"
  - "concurrency"
search_regex: "(?i)(SessionId|SessionHeader|SessionEvent|SessionPersistence|dsh\\-session|dsh\\-session\\-persistence|session/event|session/flush)"
---

# 0536. Fold the persistence interface into dsh-session — implementation context

## Open this when

dsh-session-persistence is a Service Definition package whose main concepts are already owned by dsh-session: SessionHeader, SessionEvent, SessionId, session/event, and session/flush. The package adds the abstract SessionPersistence service, the shared write coordinator, and contract helpers. Provider packages depend on it, and agent-loop has to optionally find a sibling service for resume. The capability-seam split made sense when persistence was a new swappable backend design. After the mutable summary was removed, the Service Definition package mostly wraps the session log's own storage concern.

## Source decision

Move the abstract SessionPersistence service, the coordinator, and persistence contract helpers into dsh-session. Keep JSONL and SQLite as separate backend packages that register the session-owned service. This preserves backend swappability while deleting one support package and one cross-package boundary. The implementing PR should update the capability seams guidance with the exception: persistence is not like bash or LLM because its vocabulary and lifecycle events are already the session package's core domain.

## Decision status

Rejected design. Do not implement it as written. Reuse its failure analysis, constraints, and reversal condition.

- Raw note: [rejected/simplification/2026-06-20-fold-session-persistence-interface.md](../02-notes/rejected/simplification/2026-06-20-fold-session-persistence-interface.md)
- Pinned source: [.agents/notes/rejected/simplification/2026-06-20-fold-session-persistence-interface.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/rejected/simplification/2026-06-20-fold-session-persistence-interface.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`packages/session/session-persistence/README.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/session/session-persistence/README.md) | package contract and examples | The source note names this file directly. Core file in the package named by the note: `packages/session/session-persistence`. | `named-file, named-package-member` |
| [`packages/core/session/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/index.ts) | package entry point | Core file in the package named by the note: `packages/core/session`. | `named-package-member` |
| [`packages/core/session/src/types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/types.ts) | public types and contract | Core file in the package named by the note: `packages/core/session`. Defines `SessionHeader`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/core/agent-loop/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent-loop/src/index.ts) | package entry point | Core file in the package named by the note: `packages/core/agent-loop`. | `named-package-member` |
| [`packages/core/session/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/core/session`. | `named-package-member` |
| [`packages/core/agent-loop/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent-loop/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/core/agent-loop`. | `named-package-member` |
| [`packages/session/session-persistence/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/session/session-persistence/src/index.ts) | package entry point | Core file in the package named by the note: `packages/session/session-persistence`. Defines `SessionPersistence`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/session/session-persistence/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/session/session-persistence/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/session/session-persistence`. | `named-package-member` |
| [`packages/core/session`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/packages/core/session) | package or module directory | The note names this package or capability. | `named-package` |
| [`packages/core/agent-loop`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent-loop) | package or module directory | The note names this package or capability. | `named-package` |
| [`packages/session/session-persistence`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/packages/session/session-persistence) | package or module directory | The note names this package or capability. | `named-package` |
| [`packages/core/session/README.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/README.md) | package contract and examples | Core file in the package named by the note: `packages/core/session`. | `named-package-member` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `SessionId` | `type` | [`packages/core/session/src/types.ts:22`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/types.ts#L22) | `export type SessionId = Branded<'SessionId'>` |
| `SessionId` | `function` | [`packages/core/session/src/types.ts:29`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/types.ts#L29) | `export function SessionId(id: string): SessionId {` |
| `SessionHeader` | `interface` | [`packages/core/session/src/types.ts:61`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/types.ts#L61) | `export interface SessionHeader {` |
| `SessionEvent` | `type` | [`packages/core/session/src/types.ts:404`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/types.ts#L404) | `export type SessionEvent<T extends SessionEventType = SessionEventType> = {` |
| `SessionPersistence` | `class` | [`packages/session/session-persistence/src/index.ts:84`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/session/session-persistence/src/index.ts#L84) | `export abstract class SessionPersistence extends Service {` |

### Tests and executable evidence

- [`packages/core/agent-loop/tests/resume.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent-loop/tests/resume.spec.ts) — A test under the owning area exercises or imports `SessionHeader`.
- [`packages/session/session-persistence/tests/contract.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/session/session-persistence/tests/contract.ts) — A test under the owning area exercises or imports `SessionPersistence`.
- [`packages/session/session-persistence/tests/persistence.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/session/session-persistence/tests/persistence.spec.ts) — A test under the owning area exercises or imports `SessionPersistence`.
- [`scripts/client-bundle-purity.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/client-bundle-purity.spec.ts) — Contains the exact code literal `dsh-session` named by the note.
- [`packages/feedback/message-feedback/tests/helpers.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/feedback/message-feedback/tests/helpers.ts) — Contains the exact code literal `dsh-session-persistence` named by the note.
- Source verification intent: @deepseek-ai/dsh-session-persistence is removed as a package. dsh-session exports the persistence service type, coordinator, and contract helpers. JSONL and SQLite backend packages depend on dsh-session directly. agent-loop resume uses the session-owned service key. Session persistence, shared persistence write coordinator, and package docs explain why backend implementations remain separate.

## How to read the implementation

1. Start with [`packages/session/session-persistence/README.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/session/session-persistence/README.md) because it has the strongest evidence link to the note.
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
- **Status gate** — Rejected design. Do not implement it as written. Reuse its failure analysis, constraints, and reversal condition.

### Smallest safe implementation slice

1. Express the semantic contract as Rust types and one owning boundary.
2. Add the smallest adapter or service needed by the existing loop; do not add a new model-facing tool unless the four-tool surface cannot express the job.
3. Record durable evidence before deriving summaries or UI state.
4. Add one happy-path test, one interruption test, and one replay/restart test where state survives a turn.
5. Keep the patch reversible until the source note's strongest failure case is proven in the Rust harness.

## Search handles

- Tags: `class/simplification`, `concern/boundary`, `concern/concurrency`, `concern/lifecycle`, `concern/ownership`, `concern/performance`, `concern/recovery`, `domain/build-release`, `domain/extensions`, `domain/llm`, `domain/session-state`, `domain/shell-terminal`, `domain/storage`, `domain/testing`, `lifecycle/rejected`, `mechanism/registry`
- Aliases: `SessionId`, `SessionHeader`, `SessionEvent`, `SessionPersistence`, `dsh-session`, `dsh-session-persistence`, `session/event`, `session/flush`, `agent-loop`, `@deepseek-ai/dsh-session-persistence`, `Fold the persistence interface into dsh-session`, `simplification`, `boundary`, `concurrency`
- Regex: `(?i)(SessionId|SessionHeader|SessionEvent|SessionPersistence|dsh\-session|dsh\-session\-persistence|session/event|session/flush)`

```bash
rg -n --pcre2 "(?i)(SessionId|SessionHeader|SessionEvent|SessionPersistence|dsh\\-session|dsh\\-session\\-persistence|session/event|session/flush)" source-deepseek-harness
rg -l --fixed-strings "class/simplification" 03-agent-context
```

## Connected notes

- **`source-link`** — [0007. Capability seams --- Service Definition / Service Provider / Consumer roles](0007-capability-seams-service-definition-service-provider-consumer-roles.md): The source note links to this decision directly.
- **`source-link`** — [0009. Session persistence as an abstract service over the existing `SessionEvent`](0009-session-persistence-as-an-abstract-service-over-the-existing-sessioneven.md): The source note links to this decision directly.
- **`source-link`** — [0013. Shared persistence write coordinator](0013-shared-persistence-write-coordinator.md): The source note links to this decision directly.
- **`shares-code-with`** — [0102. Reusable Session preparation before publication](0102-reusable-session-preparation-before-publication.md): Shares source implementation: `packages/core/session`, `packages/core/session/src/index.ts`.
- **`shares-code-with`** — [0085. the end-seed log boundary](0085-the-end-seed-log-boundary.md): Shares source implementation: `packages/core/agent-loop/src/index.ts`, `packages/core/session`.
- **`shares-code-with`** — [0047. Package-owned invariant service contract](0047-package-owned-invariant-service-contract.md): Shares source implementation: `packages/core/agent-loop/src/index.ts`, `packages/core/agent-loop/src/invariant.ts`.
- **`shares-code-with`** — [0063. Web input state machine, composer slots, and the slash pipeline (ui-conversation input / ui-input-trigger)](0063-web-input-state-machine-composer-slots-and-the-slash-pipeline-ui-convers.md): Shares source implementation: `packages/core/session`, `packages/core/session/src/index.ts`.
- **`shares-code-with`** — [0495. ACP snapshot tests --- record-once / replay-deterministic](0495-acp-snapshot-tests-record-once-replay-deterministic.md): Shares source implementation: `packages/core/session`, `packages/core/session/src/index.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0536-fold-the-persistence-interface-into-dsh-session.md`.
