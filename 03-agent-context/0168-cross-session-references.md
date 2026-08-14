---
id: "dsh-note-0168"
title: "Cross-session references"
status: "implemented"
class: "feature"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/feature/2026-07-21-cross-session-references.md"
implementation_evidence: "medium"
target_anchor: "sandbox capability boundary"
tags:
  - "class/feature"
  - "concern/boundary"
  - "concern/cancellation-timeout"
  - "concern/concurrency"
  - "concern/discovery-routing"
  - "concern/evidence"
  - "concern/human-control"
  - "concern/lifecycle"
  - "concern/ownership"
  - "concern/recovery"
  - "concern/schema-types"
  - "concern/trust"
  - "domain/agent-loop"
  - "domain/configuration"
  - "domain/context"
  - "domain/extensions"
  - "domain/llm"
  - "domain/observability"
  - "domain/protocols"
  - "domain/security"
  - "domain/session-state"
  - "domain/shell-terminal"
  - "domain/storage"
  - "domain/testing"
  - "domain/ui-interaction"
  - "lifecycle/implemented"
  - "mechanism/event-sourcing"
  - "mechanism/projection"
aliases:
  - "inject"
  - "UserMessage"
  - "@deepseek-ai/dsh-session-reference"
  - "ctx.sessionReferenceResolver"
  - "sessionReferenceResolver"
  - "SessionReferenceInput[]"
  - "dsh-session:<base64url(JSON.stringify(sessionId))>"
  - "ctx.sessionQuery.readSurface"
  - "dsh-compaction"
  - "as the lossless JSON escape"
  - "user/message"
  - "agent/pre-step"
  - "dsh-output-retention"
  - "SendOptions"
search_regex: "(?i)(inject|UserMessage|@deepseek\\-ai/dsh\\-session\\-reference|ctx\\.sessionReferenceResolver|sessionReferenceResolver|SessionReferenceInput\\[\\]|dsh\\-session:<base64url\\(JSON\\.stringify\\(sessionId\\)\\)>|ctx\\.sessionQuery\\.readSurface)"
---

# 0168. Cross-session references — implementation context

## Open this when

TUI users need to bring relevant work from another conversation into one new message without resuming, forking, or granting the source transcript authority over the current session. The harness already exposes exact session enumeration and raw event inspection, but every host independently parsing logs would duplicate compaction folding, filtering by cited source-event seqs, size limits, error behavior, and persistence. Encoding host markup directly into the agent message contract would also bind the core loop to one UI syntax.

## Source decision

@deepseek-ai/dsh-session-reference is one context consumer service at ctx.sessionReferenceResolver. Hosts normalize their protocol into SessionReferenceInput[] and call prepare() before delivery. The service returns detached readable content plus an optional identified, frozen UserMessage snapshot; core agent packages do not parse session URIs or read another log. dsh-session: is the canonical host-independent identifier. JSON string encoding precedes base64url so quotes, slashes, backslashes, Unicode, newlines, and every other JavaScript string value round-trip without delimiter ambiguity.

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/feature/2026-07-21-cross-session-references.md](../02-notes/implemented/feature/2026-07-21-cross-session-references.md)
- Pinned source: [.agents/notes/implemented/feature/2026-07-21-cross-session-references.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/feature/2026-07-21-cross-session-references.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`packages/core/session/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/index.ts) | package entry point | Core file in the package named by the note: `packages/core/session`. | `named-package-member` |
| [`packages/core/session/src/types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/types.ts) | public types and contract | Core file in the package named by the note: `packages/core/session`. | `named-package-member` |
| [`packages/core/session/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/core/session`. Defines `inject`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/compaction/compaction/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/compaction/compaction/src/index.ts) | package entry point | Core file in the package named by the note: `packages/compaction/compaction`. | `named-package-member` |
| [`packages/compaction/compaction/src/types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/compaction/compaction/src/types.ts) | public types and contract | Core file in the package named by the note: `packages/compaction/compaction`. | `named-package-member` |
| [`packages/util/output-retention/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/util/output-retention/src/index.ts) | package entry point | Core file in the package named by the note: `packages/util/output-retention`. | `named-package-member` |
| [`packages/compaction/compaction/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/compaction/compaction/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/compaction/compaction`. Defines `inject`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/context/session-reference/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/context/session-reference/src/index.ts) | package entry point | Core file in the package named by the note: `packages/context/session-reference`. | `named-package-member` |
| [`packages/context/session-reference/src/types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/context/session-reference/src/types.ts) | public types and contract | Core file in the package named by the note: `packages/context/session-reference`. | `named-package-member` |
| [`packages/util/output-retention/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/util/output-retention/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/util/output-retention`. Defines `inject`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/context/session-reference/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/context/session-reference/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/context/session-reference`. Defines `inject`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/core/session`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/packages/core/session) | package or module directory | The note names this package or capability. | `named-package` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `inject` | `const` | [`packages/compaction/compaction/src/invariant.ts:17`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/compaction/compaction/src/invariant.ts#L17) | `export const inject = ['invariants']` |
| `inject` | `const` | [`packages/context/session-reference/src/invariant.ts:15`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/context/session-reference/src/invariant.ts#L15) | `export const inject = ['invariants']` |
| `inject` | `const` | [`packages/core/session/src/invariant.ts:20`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/invariant.ts#L20) | `export const inject = ['invariants']` |
| `UserMessage` | `interface` | [`packages/llm/llm/src/message.ts:141`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/src/message.ts#L141) | `export interface UserMessage extends Message {` |
| `inject` | `const` | [`packages/util/output-retention/src/invariant.ts:15`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/util/output-retention/src/invariant.ts#L15) | `export const inject = ['invariants']` |

### Tests and executable evidence

- [`packages/core/session/tests/scoped.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/tests/scoped.spec.ts) — A test under the owning area exercises or imports `prepare`.
- [`packages/core/session/tests/session.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/tests/session.spec.ts) — A test under the owning area exercises or imports `prepare`.
- [`packages/compaction/compaction/tests/invariant.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/compaction/compaction/tests/invariant.spec.ts) — A test under the owning area exercises or imports `dsh-compaction`.
- [`packages/compaction/compaction/tests/compaction.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/compaction/compaction/tests/compaction.spec.ts) — A test under the owning area exercises or imports `dsh-compaction`.
- [`packages/compaction/compaction/tests/tool-pairing.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/compaction/compaction/tests/tool-pairing.spec.ts) — A test under the owning area exercises or imports `dsh-compaction`.
- [`packages/util/output-retention/tests/output-retention.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/util/output-retention/tests/output-retention.spec.ts) — A test under the owning area exercises or imports `dsh-output-retention`.
- [`packages/context/session-reference/tests/session-reference.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/context/session-reference/tests/session-reference.spec.ts) — A test under the owning area exercises or imports `sessionReferenceResolver`.
- [`apps/cli/tests/fixtures/dsh-badge/snapshot.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/cli/tests/fixtures/dsh-badge/snapshot.ts) — Contains the exact code literal `agent/pre-step` named by the note.
- Source verification intent: Unit and integration coverage pins URI round-trips and text-boundary punctuation, explicit malformed references, id/cwd/title candidate matching and ranking, failed title-observation fallback, candidate cancellation, terminal-control escaping, projection exclusions, non-recursive snapshot projection, backend-independent compact checkpoints, tag-safe framing, deduplication, self-reference, count limits, all-or-nothing reads, prompt cancellation against a non-settling storage read, independent per-source byte retention, prompt blocking, admission-time staging, send/steer placement, title isolation, missing.

## How to read the implementation

1. Start with [`packages/core/session/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/index.ts) because it has the strongest evidence link to the note.
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

- Tags: `class/feature`, `concern/boundary`, `concern/cancellation-timeout`, `concern/concurrency`, `concern/discovery-routing`, `concern/evidence`, `concern/human-control`, `concern/lifecycle`, `concern/ownership`, `concern/recovery`, `concern/schema-types`, `concern/trust`, `domain/agent-loop`, `domain/configuration`, `domain/context`, `domain/extensions`, `domain/llm`, `domain/observability`, `domain/protocols`, `domain/security`, `domain/session-state`, `domain/shell-terminal`, `domain/storage`, `domain/testing`, `domain/ui-interaction`, `lifecycle/implemented`, `mechanism/event-sourcing`, `mechanism/projection`
- Aliases: `inject`, `UserMessage`, `@deepseek-ai/dsh-session-reference`, `ctx.sessionReferenceResolver`, `sessionReferenceResolver`, `SessionReferenceInput[]`, `dsh-session:<base64url(JSON.stringify(sessionId))>`, `ctx.sessionQuery.readSurface`, `dsh-compaction`, `as the lossless JSON escape`, `user/message`, `agent/pre-step`, `dsh-output-retention`, `SendOptions`
- Regex: `(?i)(inject|UserMessage|@deepseek\-ai/dsh\-session\-reference|ctx\.sessionReferenceResolver|sessionReferenceResolver|SessionReferenceInput\[\]|dsh\-session:<base64url\(JSON\.stringify\(sessionId\)\)>|ctx\.sessionQuery\.readSurface)`

```bash
rg -n --pcre2 "(?i)(inject|UserMessage|@deepseek\\-ai/dsh\\-session\\-reference|ctx\\.sessionReferenceResolver|sessionReferenceResolver|SessionReferenceInput\\[\\]|dsh\\-session:<base64url\\(JSON\\.stringify\\(sessionId\\)\\)>|ctx\\.sessionQuery\\.readSurface)" source-deepseek-harness
rg -l --fixed-strings "class/feature" 03-agent-context
```

## Connected notes

- **`source-link`** — [0058. Separate context injection from turn execution](0058-separate-context-injection-from-turn-execution.md): The source note links to this decision directly.
- **`source-link`** — [0459. ACP as an automation-only protocol](0459-acp-as-an-automation-only-protocol.md): The source note links to this decision directly.
- **`shares-code-with`** — [0316. The human transcript projects append-origin events](0316-the-human-transcript-projects-append-origin-events.md): Shares source implementation: `packages/compaction/compaction/src/index.ts`, `packages/compaction/compaction/src/invariant.ts`.
- **`shares-code-with`** — [0518. Recallable compaction --- index checkpoints, a state checkpoint, and in-session history recall](0518-recallable-compaction-index-checkpoints-a-state-checkpoint-and-in-sessio.md): Shares source implementation: `packages/compaction/compaction/src/index.ts`, `packages/compaction/compaction/src/invariant.ts`.
- **`shares-code-with`** — [0012. Session surface --- an ordered projection over the event log](0012-session-surface-an-ordered-projection-over-the-event-log.md): Shares source implementation: `packages/compaction/compaction/src/index.ts`, `packages/compaction/compaction/src/invariant.ts`.
- **`shares-code-with`** — [0003. Event-sourced sessions with derived message history](0003-event-sourced-sessions-with-derived-message-history.md): Shares source implementation: `packages/compaction/compaction/src/index.ts`, `packages/compaction/compaction/src/invariant.ts`.
- **`shares-code-with`** — [0495. ACP snapshot tests --- record-once / replay-deterministic](0495-acp-snapshot-tests-record-once-replay-deterministic.md): Shares source implementation: `packages/core/session/src/index.ts`, `packages/core/session/src/invariant.ts`.
- **`shares-code-with`** — [0178. Web UI permission presets and approval answering](0178-web-ui-permission-presets-and-approval-answering.md): Shares source implementation: `packages/core/session/src/index.ts`, `packages/core/session/src/invariant.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0168-cross-session-references.md`.
