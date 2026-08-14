---
id: "dsh-note-0130"
title: "Multiplex concurrent ACP sessions over one connection"
status: "implemented"
class: "feature"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/feature/2026-06-14-acp-multi-session.md"
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
  - "concern/trust"
  - "domain/agent-loop"
  - "domain/build-release"
  - "domain/configuration"
  - "domain/extensions"
  - "domain/filesystem"
  - "domain/jobs-tasks"
  - "domain/llm"
  - "domain/protocols"
  - "domain/security"
  - "domain/session-state"
  - "domain/shell-terminal"
  - "domain/storage"
  - "domain/testing"
  - "domain/ui-interaction"
  - "lifecycle/implemented"
  - "mechanism/policy"
aliases:
  - "ownedRecord"
  - "AgentHandle"
  - "cwd"
  - "sandboxPolicy"
  - "Map<SessionId, SessionRecord>"
  - "agent.session.id"
  - "session/event"
  - "turn/start"
  - "turn/end"
  - "session/cancel"
  - "approval/request"
  - "job_output"
  - "job_kill"
  - "additionalDirectories"
search_regex: "(?i)(ownedRecord|AgentHandle|sandboxPolicy|Map<SessionId,[- ]SessionRecord>|agent\\.session\\.id|session/event|turn/start|turn/end)"
---

# 0130. Multiplex concurrent ACP sessions over one connection — implementation context

## Open this when

An ACP automation client can keep several conversations alive over one agent subprocess. A single-active-session bridge would force extra processes and prevent one parent controller from driving independent children over one connection. Multiplexing introduces isolation risks: committed answers, prompt completion, cancellation, permission requests, and predictable background-job ids must never cross session boundaries.

## Source decision

The ACP bridge stores live sessions in Map. Agent-scoped callbacks use ownedRecord: look up agent.session.id in that forward map and accept the record only when it owns the exact agent object, so a foreign same-id object cannot claim the session. A record owns its agent, exact disposer, and optional in-flight prompt with the durable turn number that eventually settles it. The session header owns its cwd; the bridge keeps no parallel workspace or client-capability state. Every session/event callback resolves the owning record before sending or settling anything.

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/feature/2026-06-14-acp-multi-session.md](../02-notes/implemented/feature/2026-06-14-acp-multi-session.md)
- Pinned source: [.agents/notes/implemented/feature/2026-06-14-acp-multi-session.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/feature/2026-06-14-acp-multi-session.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`packages/acp/acp/README.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/acp/acp/README.md) | package contract and examples | The source note names this file directly. Contains the exact code literal `session/cancel` named by the note. | `exact-code-occurrence, named-file` |
| [`scripts/install-lefthook.mjs`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/install-lefthook.mjs) | repository automation | Defines `ownedRecord`, a construct named by the note. | `symbol-definition` |
| [`packages/acp/acp/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/acp/acp/src/index.ts) | package entry point | Defines `ownedRecord`, a construct named by the note. | `symbol-definition` |
| [`packages/fs/tool-fs/src/edit.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/tool-fs/src/edit.ts) | runtime implementation | Defines `sandboxPolicy`, a construct named by the note. | `symbol-definition` |
| [`packages/core/agent/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent/src/index.ts) | package entry point | Defines `AgentHandle`, a construct named by the note. | `symbol-definition` |
| [`packages/fs/tool-fs/src/write.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/tool-fs/src/write.ts) | runtime implementation | Defines `sandboxPolicy`, a construct named by the note. | `symbol-definition` |
| [`packages/shell/tool-bash/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/shell/tool-bash/src/index.ts) | package entry point | Defines `sandboxPolicy`, a construct named by the note. | `symbol-definition` |
| [`packages/shell/tool-pwsh/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/shell/tool-pwsh/src/index.ts) | package entry point | Defines `sandboxPolicy`, a construct named by the note. | `symbol-definition` |
| [`packages/fs/tool-fs/src/session-cwd.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/tool-fs/src/session-cwd.ts) | runtime implementation | Defines `cwd`, a construct named by the note. | `symbol-definition` |
| [`packages/host/apiproxy/src/api-proxy.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/src/api-proxy.ts) | runtime implementation | Defines `cwd`, a construct named by the note. | `symbol-definition` |
| [`packages/fs/tool-fs-search/src/search-core.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/tool-fs-search/src/search-core.ts) | runtime implementation | Defines `cwd`, a construct named by the note. | `symbol-definition` |
| [`packages/fs/tool-str-replace-editor/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/tool-str-replace-editor/src/index.ts) | package entry point | Defines `sandboxPolicy`, a construct named by the note. | `symbol-definition` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `ownedRecord` | `const` | [`packages/acp/acp/src/index.ts:115`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/acp/acp/src/index.ts#L115) | `const ownedRecord = (agent: Agent): SessionRecord \| undefined => {` |
| `AgentHandle` | `interface` | [`packages/core/agent/src/index.ts:172`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent/src/index.ts#L172) | `export interface AgentHandle {` |
| `cwd` | `const` | [`packages/fs/tool-fs-search/src/search-core.ts:223`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/tool-fs-search/src/search-core.ts#L223) | `const cwd = exec.agent?.session.header.cwd` |
| `sandboxPolicy` | `const` | [`packages/fs/tool-fs/src/edit.ts:116`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/tool-fs/src/edit.ts#L116) | `const sandboxPolicy = await sandbox.resolvePolicy('edit', args, exec)` |
| `cwd` | `const` | [`packages/fs/tool-fs/src/session-cwd.ts:24`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/tool-fs/src/session-cwd.ts#L24) | `const cwd = exec.agent?.session.header.cwd` |
| `cwd` | `const` | [`packages/fs/tool-fs/src/session-cwd.ts:41`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/tool-fs/src/session-cwd.ts#L41) | `const cwd = policyWorkspaceRoot ?? sessionCwd(exec, requestedPath)` |
| `sandboxPolicy` | `const` | [`packages/fs/tool-fs/src/write.ts:107`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/tool-fs/src/write.ts#L107) | `const sandboxPolicy = await sandbox.resolvePolicy('write', args, exec)` |
| `sandboxPolicy` | `const` | [`packages/fs/tool-str-replace-editor/src/index.ts:247`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/tool-str-replace-editor/src/index.ts#L247) | `const sandboxPolicy = policy.resolve(exec)` |
| `cwd` | `const` | [`packages/host/apiproxy/src/api-proxy.ts:2180`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/src/api-proxy.ts#L2180) | `const cwd = workspace?.path ?? request.payload.cwd ?? defaults.cwd` |
| `cwd` | `const` | [`packages/host/apiproxy/src/api-proxy.ts:3224`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/src/api-proxy.ts#L3224) | `const cwd = session.header.cwd` |
| `sandboxPolicy` | `const` | [`packages/shell/tool-bash/src/index.ts:194`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/shell/tool-bash/src/index.ts#L194) | `const sandboxPolicy: SandboxPolicyService \| undefined = defaultMode === undefined ? undefined : ctx.get('sandboxPolicy')` |
| `sandboxPolicy` | `const` | [`packages/shell/tool-pwsh/src/index.ts:200`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/shell/tool-pwsh/src/index.ts#L200) | `const sandboxPolicy: SandboxPolicyService \| undefined = defaultMode === undefined ? undefined : ctx.get('sandboxPolicy')` |
| `ownedRecord` | `const` | [`scripts/install-lefthook.mjs:384`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/install-lefthook.mjs#L384) | `const ownedRecord = \`${String(process.pid)} ${randomUUID()}\n\`` |

### Tests and executable evidence

- [`packages/acp/acp/tests/bridge.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/acp/acp/tests/bridge.spec.ts) — A test under the owning area exercises or imports `additionalDirectories`.
- [`apps/web/tests/settings-chrome.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/settings-chrome.e2e.ts) — A test under the owning area exercises or imports `workspace-write`.
- [`apps/cli/tests/web-agent-presets.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/cli/tests/web-agent-presets.e2e.ts) — A test under the owning area exercises or imports `job_output`. A test under the owning area exercises or imports `job_kill`.
- [`apps/web/tests/shipped-composition.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/shipped-composition.e2e.ts) — A test under the owning area exercises or imports `job_output`. A test under the owning area exercises or imports `job_kill`.
- [`packages/jobs/jobs-local/tests/jobs.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/jobs/jobs-local/tests/jobs.spec.ts) — A test under the owning area exercises or imports `job_kill`.
- [`packages/shell/tool-bash/tests/tools.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/shell/tool-bash/tests/tools.spec.ts) — A test under the owning area exercises or imports `job_output`. A test under the owning area exercises or imports `job_kill`.
- [`packages/shell/tool-pwsh/tests/tools.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/shell/tool-pwsh/tests/tools.spec.ts) — A test under the owning area exercises or imports `job_output`. A test under the owning area exercises or imports `job_kill`.
- [`packages/jobs/tool-jobs/tests/tool-jobs.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/jobs/tool-jobs/tests/tool-jobs.spec.ts) — A test under the owning area exercises or imports `job_output`. A test under the owning area exercises or imports `job_kill`.
- Source verification intent: The multi-session suite drives concurrent sessions through routed committed answers, independent in-flight prompts, targeted cancellation, and shared teardown; the approval and output-boundary suites cover permission routing and exact-agent rejection. Tool-bash tests prove one session cannot read or kill another session's background job.

## How to read the implementation

1. Start with [`packages/acp/acp/README.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/acp/acp/README.md) because it has the strongest evidence link to the note.
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

- Tags: `class/feature`, `concern/boundary`, `concern/cancellation-timeout`, `concern/concurrency`, `concern/discovery-routing`, `concern/evidence`, `concern/human-control`, `concern/lifecycle`, `concern/ownership`, `concern/recovery`, `concern/trust`, `domain/agent-loop`, `domain/build-release`, `domain/configuration`, `domain/extensions`, `domain/filesystem`, `domain/jobs-tasks`, `domain/llm`, `domain/protocols`, `domain/security`, `domain/session-state`, `domain/shell-terminal`, `domain/storage`, `domain/testing`, `domain/ui-interaction`, `lifecycle/implemented`, `mechanism/policy`
- Aliases: `ownedRecord`, `AgentHandle`, `cwd`, `sandboxPolicy`, `Map<SessionId, SessionRecord>`, `agent.session.id`, `session/event`, `turn/start`, `turn/end`, `session/cancel`, `approval/request`, `job_output`, `job_kill`, `additionalDirectories`
- Regex: `(?i)(ownedRecord|AgentHandle|sandboxPolicy|Map<SessionId,[- ]SessionRecord>|agent\.session\.id|session/event|turn/start|turn/end)`

```bash
rg -n --pcre2 "(?i)(ownedRecord|AgentHandle|sandboxPolicy|Map<SessionId,[- ]SessionRecord>|agent\\.session\\.id|session/event|turn/start|turn/end)" source-deepseek-harness
rg -l --fixed-strings "class/feature" 03-agent-context
```

## Connected notes

- **`source-link`** — [0022. Resolve filesystem paths against the caller's session cwd](0022-resolve-filesystem-paths-against-the-caller-s-session-cwd.md): The source note links to this decision directly.
- **`source-link`** — [0459. ACP as an automation-only protocol](0459-acp-as-an-automation-only-protocol.md): The source note links to this decision directly.
- **`shares-code-with`** — [0557. Agent Client Protocol (ACP) support --- drive the coding agent from external editors](0557-agent-client-protocol-acp-support-drive-the-coding-agent-from-external-e.md): Shares source implementation: `packages/acp/acp/README.md`, `packages/acp/acp/src/index.ts`.
- **`shares-code-with`** — [0528. Prune dead public API and result fields](0528-prune-dead-public-api-and-result-fields.md): Shares source implementation: `packages/core/agent/src/index.ts`, `packages/fs/tool-fs/src/edit.ts`.
- **`shares-code-with`** — [0144. The approval seam --- one-shot permission decisions over a waterfall of answerers](0144-the-approval-seam-one-shot-permission-decisions-over-a-waterfall-of-answ.md): Shares source implementation: `packages/acp/acp/src/index.ts`, `packages/core/agent/src/index.ts`.
- **`shares-code-with`** — [0135. ACP subagent backend (out-of-process delegation)](0135-acp-subagent-backend-out-of-process-delegation.md): Shares source implementation: `packages/acp/acp/src/index.ts`, `packages/core/agent/src/index.ts`.
- **`shares-code-with`** — [0155. Cross-family file sandbox --- one policy home, a sandboxed fs provider, and fs escalation parity](0155-cross-family-file-sandbox-one-policy-home-a-sandboxed-fs-provider-and-fs.md): Shares source implementation: `packages/fs/tool-fs/src/edit.ts`, `packages/fs/tool-fs/src/write.ts`.
- **`shares-code-with`** — [0200. Continuable subagents](0200-continuable-subagents.md): Shares source implementation: `packages/acp/acp/src/index.ts`, `packages/core/agent/src/index.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0130-multiplex-concurrent-acp-sessions-over-one-connection.md`.
