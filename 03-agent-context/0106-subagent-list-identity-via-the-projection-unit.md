---
id: "dsh-note-0106"
title: "Subagent list identity via the projection unit"
status: "implemented"
class: "architecture"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/architecture/2026-08-06-subagent-list-identity-projection.md"
implementation_evidence: "high"
target_anchor: "Rust provider-adapter boundary"
tags:
  - "class/architecture"
  - "concern/boundary"
  - "concern/cancellation-timeout"
  - "concern/compatibility"
  - "concern/concurrency"
  - "concern/discovery-routing"
  - "concern/evidence"
  - "concern/lifecycle"
  - "concern/ownership"
  - "concern/performance"
  - "concern/recovery"
  - "concern/schema-types"
  - "domain/agent-loop"
  - "domain/build-release"
  - "domain/configuration"
  - "domain/extensions"
  - "domain/llm"
  - "domain/multi-agent"
  - "domain/observability"
  - "domain/session-state"
  - "domain/storage"
  - "domain/testing"
  - "domain/web-retrieval"
  - "lifecycle/implemented"
  - "mechanism/generation"
  - "mechanism/policy"
  - "mechanism/projection"
  - "mechanism/registry"
aliases:
  - "readEvent"
  - "activity"
  - "subagent"
  - "seq"
  - "reason"
  - "inspect"
  - "createdAt"
  - "view"
  - "projectionsFor"
  - "hasSubagentOwner"
  - "inspectServable"
  - "sessionQuery"
  - "traceSession"
  - "Config"
search_regex: "(?i)(readEvent|activity|subagent|reason|inspect|createdAt|view|projectionsFor)"
---

# 0106. Subagent list identity via the projection unit — implementation context

## Open this when

Before the rewrite, SubagentRuntime.listChildren ran two full-log materializations --- listEvents plus readEvent --- on every listing for each direct child with header.origin === 'subagent', each materialization accompanied by a full-log structuredClone, all to fold two fields, mode and label, out of the descriptor event. The descriptor's position in the log is not fixed --- the fork prefix is arbitrarily long, and zstd-compressed frames carry no seq index --- so there is no shortcut to locating it; this path had no cache whatsoever, and its cost amplifies with transcript length × child count × listing.

## Source decision

mode and label are folded by the new subagent projection unit (pure identity, two arms), and the unit is the sole authority over the fold rules; listChildren no longer depends on session-query --- enumeration is a subagent-owned live-preferred merge, and value retrieval walks a three-rung compute-and-discard ladder: a live child synchronously reads the registry's existing watermark cache (zero log reads); a cold child first asks the optional sessionProjectionCache checkpoint, and a served identity that passes the seq gate is final; otherwise it pays one full persistence.inspect read plus one registry.restore.

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/architecture/2026-08-06-subagent-list-identity-projection.md](../02-notes/implemented/architecture/2026-08-06-subagent-list-identity-projection.md)
- Pinned source: [.agents/notes/implemented/architecture/2026-08-06-subagent-list-identity-projection.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/architecture/2026-08-06-subagent-list-identity-projection.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`vendor/cordis/src/events.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/cordis/src/events.ts) | runtime implementation | The source note names this file directly. | `named-file` |
| [`packages/subagent/subagent/src/projection.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/subagent/subagent/src/projection.ts) | runtime implementation | The source note names this file directly. | `named-file` |
| [`packages/subagent/subagent/src/list-children.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/subagent/subagent/src/list-children.ts) | runtime implementation | The source note names this file directly. Core file in the package named by the note: `packages/subagent/subagent`. | `named-file, named-package-member, symbol-definition` |
| [`packages/subagent/subagent/src/projection-types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/subagent/subagent/src/projection-types.ts) | runtime implementation | The source note names this file directly. | `named-file` |
| [`packages/subagent/subagent/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/subagent/subagent/src/index.ts) | package entry point | Core file in the package named by the note: `packages/subagent/subagent`. | `named-package-member` |
| [`packages/subagent/subagent/src/types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/subagent/subagent/src/types.ts) | public types and contract | Core file in the package named by the note: `packages/subagent/subagent`. | `named-package-member` |
| [`packages/subagent/subagent/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/subagent/subagent/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/subagent/subagent`. Defines `apply`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/subagent/subagent/src/descriptor.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/subagent/subagent/src/descriptor.ts) | runtime implementation | Core file in the package named by the note: `packages/subagent/subagent`. Defines `snapshot`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/subagent/subagent/src/continuation.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/subagent/subagent/src/continuation.ts) | runtime implementation | Core file in the package named by the note: `packages/subagent/subagent`. Defines `parentSession`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/session/session-projection/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/session/session-projection/src/index.ts) | package entry point | Core file in the package named by the note: `packages/session/session-projection`. Defines `ProjectionDefinition`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/session/session-projection/src/types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/session/session-projection/src/types.ts) | public types and contract | Core file in the package named by the note: `packages/session/session-projection`. | `named-package-member` |
| [`packages/session/session-projection/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/session/session-projection/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/session/session-projection`. Defines `apply`, a construct named by the note. | `named-package-member, symbol-definition` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `readEvent` | `function` | [`.github/issue-management/policy.mjs:689`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.github/issue-management/policy.mjs#L689) | `function readEvent() {` |
| `activity` | `const` | [`packages/client/runtime/src/client/sessions/manager.ts:930`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/runtime/src/client/sessions/manager.ts#L930) | `const activity = running ? 'running' as const : 'inactive' as const` |
| `subagent` | `const` | [`packages/client/ui-subagent/src/client/index.ts:45`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-subagent/src/client/index.ts#L45) | `const subagent = owner.session?.subagent` |
| `seq` | `const` | [`packages/core/session/src/index.ts:233`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/index.ts#L233) | `const seq = event['seq']` |
| `reason` | `const` | [`packages/core/tools/src/index.ts:749`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/index.ts#L749) | `const reason = guard(exec)` |
| `inspect` | `const` | [`packages/extensions/cordis-client-runner/src/client/index.ts:189`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/extensions/cordis-client-runner/src/client/index.ts#L189) | `const inspect = new ClientCordisInspectRegistry({` |
| `createdAt` | `const` | [`packages/goal/goal/src/fold.ts:160`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/goal/goal/src/fold.ts#L160) | `const createdAt = nonNegativeInteger(value['createdAt'], 'createdAt')` |
| `view` | `const` | [`packages/goal/goal/src/index.ts:535`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/goal/goal/src/index.ts#L535) | `const view = this.view(cache)` |
| `projectionsFor` | `function` | [`packages/host/apiproxy/src/api-proxy.ts:841`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/src/api-proxy.ts#L841) | `function projectionsFor(ctx: Context, session: Session): SessionProjectionsBlock \| undefined {` |
| `hasSubagentOwner` | `const` | [`packages/host/apiproxy/src/api-proxy.ts:1250`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/src/api-proxy.ts#L1250) | `const hasSubagentOwner = (` |
| `inspectServable` | `const` | [`packages/host/apiproxy/src/api-proxy.ts:1256`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/src/api-proxy.ts#L1256) | `const inspectServable = (sessionId: SessionId): Promise<{ meta: SessionHeader; events: SessionEvent[] }> =>` |
| `sessionQuery` | `const` | [`packages/host/apiproxy/src/api-proxy.ts:2043`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/src/api-proxy.ts#L2043) | `const sessionQuery = ctx.get('sessionQuery')` |
| `traceSession` | `function` | [`packages/session-query/session-query/src/tracing.ts:113`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/session-query/session-query/src/tracing.ts#L113) | `export function traceSession(` |
| `Config` | `interface` | [`packages/session/session-projection-cache/src/index.ts:42`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/session/session-projection-cache/src/index.ts#L42) | `export interface Config {` |
| `Config` | `const` | [`packages/session/session-projection-cache/src/index.ts:49`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/session/session-projection-cache/src/index.ts#L49) | `export const Config: z<Config> = z.object({` |
| `apply` | `const` | [`packages/session/session-projection-cache/src/invariant.ts:33`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/session/session-projection-cache/src/invariant.ts#L33) | `export const apply = (ctx: Context): Promise<() => void> =>` |

### Tests and executable evidence

- [`packages/subagent/subagent/tests/list-children.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/subagent/subagent/tests/list-children.spec.ts) — The source note names this file directly. A test under the owning area exercises or imports `listChildren`.
- [`packages/host/apiproxy/tests/api-proxy-fork.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/tests/api-proxy-fork.spec.ts) — A test under the owning area exercises or imports `sessionQuery`.
- [`packages/host/apiproxy/tests/session-export.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/tests/session-export.spec.ts) — A test under the owning area exercises or imports `sessionQuery`.
- [`packages/client/runtime/tests/manager.client.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/runtime/tests/manager.client.spec.ts) — A test under the owning area exercises or imports `activity`.
- [`packages/subagent/subagent/tests/continuation.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/subagent/subagent/tests/continuation.spec.ts) — A test under the owning area exercises or imports `NOT_RESUMABLE`.
- [`packages/host/apiproxy/tests/api-proxy-search.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/tests/api-proxy-search.spec.ts) — A test under the owning area exercises or imports `sessionQuery`.
- [`packages/session-query/session-query/tests/tracing.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/session-query/session-query/tests/tracing.spec.ts) — A test under the owning area exercises or imports `traceSession`.
- [`packages/session/session-projection/tests/registry.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/session/session-projection/tests/registry.spec.ts) — A test under the owning area exercises or imports `ProjectionDefinition`. A test under the owning area exercises or imports `stateVersion`.
- Source verification intent: packages/subagent/subagent/tests/list-children.spec.ts is rewritten to this contract: live-only listing without persistence, query services, or the continuation runtime; with the registry absent, even zero children loudly report SUBAGENT_CONTROL_PROJECTIONS_UNAVAILABLE; a live child incurs zero inspect throughout while a cold child incurs exactly one per listing; multiple descriptors resolve last-wins to the final one; corrupt payloads and unknown versions fold to corrupt; a cold-read failure maps to unavailable and retries on the next listing; the ancestor descriptor in a fork seed forms a row under.

## How to read the implementation

1. Start with [`vendor/cordis/src/events.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/cordis/src/events.ts) because it has the strongest evidence link to the note.
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

### Smallest safe implementation slice

1. Express the semantic contract as Rust types and one owning boundary.
2. Add the smallest adapter or service needed by the existing loop; do not add a new model-facing tool unless the four-tool surface cannot express the job.
3. Record durable evidence before deriving summaries or UI state.
4. Add one happy-path test, one interruption test, and one replay/restart test where state survives a turn.
5. Keep the patch reversible until the source note's strongest failure case is proven in the Rust harness.

## Search handles

- Tags: `class/architecture`, `concern/boundary`, `concern/cancellation-timeout`, `concern/compatibility`, `concern/concurrency`, `concern/discovery-routing`, `concern/evidence`, `concern/lifecycle`, `concern/ownership`, `concern/performance`, `concern/recovery`, `concern/schema-types`, `domain/agent-loop`, `domain/build-release`, `domain/configuration`, `domain/extensions`, `domain/llm`, `domain/multi-agent`, `domain/observability`, `domain/session-state`, `domain/storage`, `domain/testing`, `domain/web-retrieval`, `lifecycle/implemented`, `mechanism/generation`, `mechanism/policy`, `mechanism/projection`, `mechanism/registry`
- Aliases: `readEvent`, `activity`, `subagent`, `seq`, `reason`, `inspect`, `createdAt`, `view`, `projectionsFor`, `hasSubagentOwner`, `inspectServable`, `sessionQuery`, `traceSession`, `Config`
- Regex: `(?i)(readEvent|activity|subagent|reason|inspect|createdAt|view|projectionsFor)`

```bash
rg -n --pcre2 "(?i)(readEvent|activity|subagent|reason|inspect|createdAt|view|projectionsFor)" source-deepseek-harness
rg -l --fixed-strings "class/architecture" 03-agent-context
```

## Connected notes

- **`source-link`** — [0102. Reusable Session preparation before publication](0102-reusable-session-preparation-before-publication.md): The source note links to this decision directly.
- **`source-link`** — [0173. Durable subagent catalog and list_agents](0173-durable-subagent-catalog-and-list-agents.md): The source note links to this decision directly.
- **`source-link`** — [0198. Web subagent catalog and human continuation](0198-web-subagent-catalog-and-human-continuation.md): The source note links to this decision directly.
- **`source-link`** — [0511. Session projections and command lifecycle logging](0511-session-projections-and-command-lifecycle-logging.md): The source note links to this decision directly.
- **`shares-code-with`** — [0148. Background subagent tasks](0148-background-subagent-tasks.md): Shares source implementation: `packages/subagent/subagent/src/continuation.ts`, `packages/subagent/subagent/src/descriptor.ts`.
- **`shares-code-with`** — [0184. In-process subagent policy inheritance --- the child starts under the parent's sandbox override](0184-in-process-subagent-policy-inheritance-the-child-starts-under-the-parent.md): Shares source implementation: `packages/subagent/subagent/src/continuation.ts`, `packages/subagent/subagent/src/index.ts`.
- **`shares-code-with`** — [0285. Delegated subagents run with approvals pinned to `'never'`](0285-delegated-subagents-run-with-approvals-pinned-to-never.md): Shares source implementation: `packages/subagent/subagent/src/continuation.ts`, `packages/subagent/subagent/src/index.ts`.
- **`shares-code-with`** — [0354. Code Mode collapses the executor, not just the wire](0354-code-mode-collapses-the-executor-not-just-the-wire.md): Shares source implementation: `packages/subagent/subagent/src/continuation.ts`, `packages/subagent/subagent/src/index.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0106-subagent-list-identity-via-the-projection-unit.md`.
