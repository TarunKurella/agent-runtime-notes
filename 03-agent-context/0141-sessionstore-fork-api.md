---
id: "dsh-note-0141"
title: "SessionStore fork API"
status: "implemented"
class: "feature"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/feature/2026-06-30-session-store-fork-api.md"
implementation_evidence: "medium"
target_anchor: "read/write/patch filesystem capability"
tags:
  - "class/feature"
  - "concern/boundary"
  - "concern/discovery-routing"
  - "concern/lifecycle"
  - "concern/ownership"
  - "concern/recovery"
  - "concern/schema-types"
  - "domain/agent-loop"
  - "domain/extensions"
  - "domain/filesystem"
  - "domain/llm"
  - "domain/multi-agent"
  - "domain/protocols"
  - "domain/session-state"
  - "domain/storage"
  - "domain/testing"
  - "domain/web-retrieval"
  - "lifecycle/implemented"
  - "mechanism/event-sourcing"
  - "mechanism/policy"
  - "mechanism/projection"
  - "mechanism/registry"
aliases:
  - "sessions"
  - "snapshot"
  - "seq"
  - "SessionStore"
  - "cwd"
  - "childSessionId"
  - "boundary"
  - "parentSession"
  - "ctx.sessions.create"
  - "turn/start"
  - "step/start"
  - "dsh-subagent-fork-in-process"
  - "dsh-session"
  - "ctx.sessions"
search_regex: "(?i)(sessions|snapshot|SessionStore|childSessionId|boundary|parentSession|ctx\\.sessions\\.create|turn/start)"
---

# 0141. SessionStore fork API — implementation context

## Open this when

The event-sourced session log already has the primitive a fork needs: create a new session with a seed event prefix, then derive model history from that seeded log exactly as replay does. That primitive is intentionally low-level: ctx.sessions.create(id, { seed, meta }) accepts any valid seed, but ordinary live-session branching needs policy around which prefix can be copied, which metadata is stamped on the child, and how errors are classified. The semantic hazard is the fork boundary. A valid user-visible fork seed must be contiguous and end outside an active turn.

## Source decision

dsh-session owns ordinary live-session forking directly on ctx.sessions. There is no separate dsh-session-fork package or ctx.sessionFork service: the API has no independent backend, event vocabulary, lifecycle, or persistence behavior, and all durable work delegates to the existing session store and persistence backends. The store exposes one operation: boundary is the inclusive source event seq to copy through. When omitted, it defaults to the source session's current last event; on an empty source, omitted boundary creates an empty child.

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/feature/2026-06-30-session-store-fork-api.md](../02-notes/implemented/feature/2026-06-30-session-store-fork-api.md)
- Pinned source: [.agents/notes/implemented/feature/2026-06-30-session-store-fork-api.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/feature/2026-06-30-session-store-fork-api.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`packages/core/session/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/index.ts) | package entry point | Core file in the package named by the note: `packages/core/session`. Defines `seq`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/core/session/src/types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/types.ts) | public types and contract | Core file in the package named by the note: `packages/core/session`. | `named-package-member` |
| [`packages/core/session/src/repair.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/repair.ts) | runtime implementation | Core file in the package named by the note: `packages/core/session`. Defines `seq`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/core/session/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/core/session`. | `named-package-member` |
| [`packages/subagent/subagent-fork-in-process/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/subagent/subagent-fork-in-process/src/index.ts) | package entry point | Core file in the package named by the note: `packages/subagent/subagent-fork-in-process`. | `named-package-member` |
| [`packages/subagent/subagent-fork-in-process/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/subagent/subagent-fork-in-process/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/subagent/subagent-fork-in-process`. | `named-package-member` |
| [`packages/core/session`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/packages/core/session) | package or module directory | The note names this package or capability. | `named-package` |
| [`packages/subagent/subagent-fork-in-process`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/packages/subagent/subagent-fork-in-process) | package or module directory | The note names this package or capability. | `named-package` |
| [`packages/acp/acp/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/acp/acp/src/index.ts) | package entry point | Defines `sessions`, a construct named by the note. | `symbol-definition` |
| [`packages/sdk/server/src/server.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/sdk/server/src/server.ts) | runtime implementation | Defines `parentSession`, a construct named by the note. | `symbol-definition` |
| [`packages/fs/tool-fs/src/session-cwd.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/tool-fs/src/session-cwd.ts) | runtime implementation | Defines `cwd`, a construct named by the note. | `symbol-definition` |
| [`packages/host/apiproxy/src/api-proxy.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/src/api-proxy.ts) | runtime implementation | Defines `boundary`, a construct named by the note. Defines `childSessionId`, a construct named by the note. | `symbol-definition` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `sessions` | `const` | [`packages/acp/acp/src/index.ts:110`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/acp/acp/src/index.ts#L110) | `const sessions = new Map<SessionId, SessionRecord>()` |
| `snapshot` | `const` | [`packages/core/session/src/index.ts:154`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/index.ts#L154) | `const snapshot = snapshotJsonValue(input)` |
| `seq` | `const` | [`packages/core/session/src/index.ts:233`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/index.ts#L233) | `const seq = event['seq']` |
| `snapshot` | `const` | [`packages/core/session/src/index.ts:519`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/index.ts#L519) | `const snapshot = mode === 'restore' ? source : snapshotJsonValue(source)` |
| `SessionStore` | `class` | [`packages/core/session/src/index.ts:792`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/index.ts#L792) | `export class SessionStore extends Service {` |
| `seq` | `let` | [`packages/core/session/src/repair.ts:85`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/repair.ts#L85) | `let seq = last.seq + 1` |
| `cwd` | `const` | [`packages/fs/tool-fs/src/session-cwd.ts:24`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/tool-fs/src/session-cwd.ts#L24) | `const cwd = exec.agent?.session.header.cwd` |
| `childSessionId` | `const` | [`packages/host/apiproxy/src/api-proxy.ts:907`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/src/api-proxy.ts#L907) | `const childSessionId = request.payload.childSessionId` |
| `boundary` | `const` | [`packages/host/apiproxy/src/api-proxy.ts:2386`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/src/api-proxy.ts#L2386) | `const boundary = anchoredBoundary` |
| `parentSession` | `const` | [`packages/sdk/server/src/server.ts:79`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/sdk/server/src/server.ts#L79) | `const parentSession = session.header.parentSession` |

### Tests and executable evidence

- [`packages/core/session/tests/fork.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/tests/fork.spec.ts) — A test under the owning area exercises or imports `parentSession`. A test under the owning area exercises or imports `seedLength`.
- [`packages/core/session/tests/session.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/tests/session.spec.ts) — A test under the owning area exercises or imports `parentSession`. A test under the owning area exercises or imports `seedLength`.
- [`packages/host/apiproxy/tests/rpc-schemas.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/tests/rpc-schemas.spec.ts) — A test under the owning area exercises or imports `childSessionId`.
- [`packages/host/apiproxy/tests/fetch-carrier.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/tests/fetch-carrier.spec.ts) — A test under the owning area exercises or imports `childSessionId`.
- [`packages/host/apiproxy/tests/client-handler.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/tests/client-handler.spec.ts) — A test under the owning area exercises or imports `childSessionId`.
- [`packages/host/apiproxy/tests/api-proxy-subagents.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/tests/api-proxy-subagents.spec.ts) — A test under the owning area exercises or imports `childSessionId`.
- [`packages/subagent/subagent-fork-in-process/tests/subagent-fork-in-process.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/subagent/subagent-fork-in-process/tests/subagent-fork-in-process.spec.ts) — A test under the owning area exercises or imports `dsh-subagent-fork-in-process`.
- [`scripts/client-bundle-purity.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/client-bundle-purity.spec.ts) — Contains the exact code literal `dsh-session` named by the note.

## How to read the implementation

1. Start with [`packages/core/session/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/index.ts) because it has the strongest evidence link to the note.
2. Read public types and entry points before implementation details. Write down the contract in plain language.
3. Follow the named constructs into their callers and tests. Confirm which component can actually enforce the rule.
4. Reconstruct one failure path: cancellation, crash, retry, stale state, partial write, or incompatible provider input—whichever the note is protecting against.
5. Only then map the contract into the current Rust repository. Target paths must come from that repository, never from guesswork in this wiki.

## Rust harness port

**Likely integration point:** read/write/patch filesystem capability.

Preserve the behavior and ownership rule. Do not mechanically translate Cordis, TypeScript declaration merging, Node streams, or package topology into Rust.

Relevant standing rules:

- **`rust/append-log`** — Keep durable facts append-only and derive views from them. Separate the synchronous fact boundary from asynchronous persistence.
- **`rust/capability-security`** — Make granted capabilities explicit. On the Windows no-admin path, probe support at runtime and fail closed rather than silently running unsandboxed.
- **`rust/four-tool-surface`** — Preserve the four model tools: read, write, patch, and exec. Reuse internal capabilities without automatically exposing new model-facing tools.
- **`rust/newtype-ids`** — Represent identity domains with Rust newtypes; do not pass bare strings across session, call, turn, step, and agent boundaries.
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

- Tags: `class/feature`, `concern/boundary`, `concern/discovery-routing`, `concern/lifecycle`, `concern/ownership`, `concern/recovery`, `concern/schema-types`, `domain/agent-loop`, `domain/extensions`, `domain/filesystem`, `domain/llm`, `domain/multi-agent`, `domain/protocols`, `domain/session-state`, `domain/storage`, `domain/testing`, `domain/web-retrieval`, `lifecycle/implemented`, `mechanism/event-sourcing`, `mechanism/policy`, `mechanism/projection`, `mechanism/registry`
- Aliases: `sessions`, `snapshot`, `seq`, `SessionStore`, `cwd`, `childSessionId`, `boundary`, `parentSession`, `ctx.sessions.create`, `turn/start`, `step/start`, `dsh-subagent-fork-in-process`, `dsh-session`, `ctx.sessions`
- Regex: `(?i)(sessions|snapshot|SessionStore|childSessionId|boundary|parentSession|ctx\.sessions\.create|turn/start)`

```bash
rg -n --pcre2 "(?i)(sessions|snapshot|SessionStore|childSessionId|boundary|parentSession|ctx\\.sessions\\.create|turn/start)" source-deepseek-harness
rg -l --fixed-strings "class/feature" 03-agent-context
```

## Connected notes

- **`source-link`** — [0134. Subagent capability seam](0134-subagent-capability-seam.md): The source note links to this decision directly.
- **`source-link`** — [0497. Persist the seed boundary so fork-child replay routes correctly](0497-persist-the-seed-boundary-so-fork-child-replay-routes-correctly.md): The source note links to this decision directly.
- **`shares-code-with`** — [0450. Unify the agent id and the session id](0450-unify-the-agent-id-and-the-session-id.md): Shares source implementation: `packages/acp/acp/src/index.ts`, `packages/core/session`.
- **`shares-code-with`** — [0063. Web input state machine, composer slots, and the slash pipeline (ui-conversation input / ui-input-trigger)](0063-web-input-state-machine-composer-slots-and-the-slash-pipeline-ui-convers.md): Shares source implementation: `packages/core/session`, `packages/core/session/src/index.ts`.
- **`shares-code-with`** — [0331. Resume selector folds titles only](0331-resume-selector-folds-titles-only.md): Shares source implementation: `packages/acp/acp/src/index.ts`, `packages/core/session`.
- **`shares-code-with`** — [0085. the end-seed log boundary](0085-the-end-seed-log-boundary.md): Shares source implementation: `packages/core/session`, `packages/core/session/src/index.ts`.
- **`shares-code-with`** — [0446. Drop the mutable session summary](0446-drop-the-mutable-session-summary.md): Shares source implementation: `packages/core/session`, `packages/core/session/src/index.ts`.
- **`shares-code-with`** — [0061. Web client Agent-scope parity model and the provisioning channel (agents/scope / blank reuse / provide)](0061-web-client-agent-scope-parity-model-and-the-provisioning-channel-agents.md): Shares source implementation: `packages/core/session`, `packages/core/session/src/index.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0141-sessionstore-fork-api.md`.
