---
id: "dsh-note-0178"
title: "Web UI permission presets and approval answering"
status: "implemented"
class: "feature"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/feature/2026-07-23-web-permission-and-approval.md"
implementation_evidence: "medium"
target_anchor: "sandbox capability boundary"
tags:
  - "class/feature"
  - "concern/boundary"
  - "concern/cancellation-timeout"
  - "concern/concurrency"
  - "concern/discovery-routing"
  - "concern/human-control"
  - "concern/lifecycle"
  - "concern/ownership"
  - "concern/recovery"
  - "concern/schema-types"
  - "concern/trust"
  - "domain/agent-loop"
  - "domain/build-release"
  - "domain/configuration"
  - "domain/filesystem"
  - "domain/llm"
  - "domain/protocols"
  - "domain/security"
  - "domain/session-state"
  - "domain/shell-terminal"
  - "domain/storage"
  - "domain/testing"
  - "domain/ui-interaction"
  - "domain/web-retrieval"
  - "lifecycle/implemented"
  - "mechanism/event-sourcing"
  - "mechanism/generation"
  - "mechanism/policy"
  - "mechanism/registry"
  - "mechanism/streaming"
aliases:
  - "PendingWait"
  - "composer"
  - "ApprovalPanel"
  - "permissions"
  - "PendingApproval"
  - "createApiProxy"
  - "decided"
  - "cancelled"
  - "ApprovalResponsePayload"
  - "RpcMethodMap"
  - "sandbox"
  - "approval"
  - "PermissionSelect"
  - "hasOpenTurn"
search_regex: "(?i)(PendingWait|composer|ApprovalPanel|permissions|PendingApproval|createApiProxy|decided|cancelled)"
---

# 0178. Web UI permission presets and approval answering — implementation context

## Open this when

The web host booted an unconfined agent: bootHost composed dsh-bash-local and dsh-fs-local, so every web session ran with full file access, no approval channel, and no permission control --- while the ACP composition had shipped the complete sandboxed product path (sandbox provider + policy home + confined shell/fs + approval + presets) for months.

## Source decision

The web host composes the same sandboxed product path as the acp-agent composition: dsh-sandbox-local, dsh-sandbox-policy, dsh-bash-sandbox, dsh-fs-sandbox, dsh-user-approval, and dsh-permission-presets, with BootHostOptions.sandbox supplying the deployment defaults (mode, default workspace-write; approvalPolicy, default ask). createApiProxy owns the approval pending registry.

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/feature/2026-07-23-web-permission-and-approval.md](../02-notes/implemented/feature/2026-07-23-web-permission-and-approval.md)
- Pinned source: [.agents/notes/implemented/feature/2026-07-23-web-permission-and-approval.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/feature/2026-07-23-web-permission-and-approval.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`packages/fs/fs-local/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/fs-local/src/index.ts) | package entry point | Core file in the package named by the note: `packages/fs/fs-local`. | `named-package-member` |
| [`packages/core/session/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/index.ts) | package entry point | Core file in the package named by the note: `packages/core/session`. | `named-package-member` |
| [`packages/core/session/src/types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/types.ts) | public types and contract | Core file in the package named by the note: `packages/core/session`. | `named-package-member` |
| [`packages/fs/fs-sandbox/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/fs-sandbox/src/index.ts) | package entry point | Core file in the package named by the note: `packages/fs/fs-sandbox`. | `named-package-member` |
| [`packages/fs/fs-local/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/fs-local/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/fs/fs-local`. | `named-package-member` |
| [`packages/sandbox/sandbox/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/sandbox/sandbox/src/index.ts) | package entry point | Core file in the package named by the note: `packages/sandbox/sandbox`. | `named-package-member` |
| [`packages/core/session/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/core/session`. | `named-package-member` |
| [`packages/shell/bash-local/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/shell/bash-local/src/index.ts) | package entry point | Core file in the package named by the note: `packages/shell/bash-local`. | `named-package-member` |
| [`packages/fs/fs-sandbox/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/fs-sandbox/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/fs/fs-sandbox`. | `named-package-member` |
| [`packages/shell/bash-sandbox/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/shell/bash-sandbox/src/index.ts) | package entry point | Core file in the package named by the note: `packages/shell/bash-sandbox`. | `named-package-member` |
| [`packages/sandbox/sandbox/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/sandbox/sandbox/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/sandbox/sandbox`. | `named-package-member` |
| [`packages/shell/bash-local/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/shell/bash-local/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/shell/bash-local`. | `named-package-member` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `PendingWait` | `class` | [`packages/client/runtime/src/client/sessions/pending.ts:34`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/runtime/src/client/sessions/pending.ts#L34) | `export class PendingWait<K extends PendingKind = PendingKind> {` |
| `composer` | `const` | [`packages/client/ui-conversation/src/client/chat/ChatView.tsx:55`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/src/client/chat/ChatView.tsx#L55) | `const composer = scrollport.querySelector<HTMLElement>('[data-composer-seat]')` |
| `ApprovalPanel` | `function` | [`packages/client/ui-conversation/src/client/skeleton/ApprovalPanel.tsx:41`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/src/client/skeleton/ApprovalPanel.tsx#L41) | `export function ApprovalPanel(props: ApprovalComposerProps) {` |
| `permissions` | `const` | [`packages/client/ui-conversation/src/client/skeleton/InputBar.tsx:123`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/src/client/skeleton/InputBar.tsx#L123) | `const permissions = useProjection('permissions')` |
| `PendingApproval` | `interface` | [`packages/host/apiproxy/src/api-proxy.ts:679`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/src/api-proxy.ts#L679) | `interface PendingApproval {` |
| `createApiProxy` | `function` | [`packages/host/apiproxy/src/api-proxy.ts:1106`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/src/api-proxy.ts#L1106) | `export function createApiProxy(ctx: Context, defaults: ApiProxyDefaults): ApiProxy {` |
| `decided` | `const` | [`packages/host/apiproxy/src/api-proxy.ts:1437`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/src/api-proxy.ts#L1437) | `const decided = new Set<ApprovalRequestId>()` |
| `cancelled` | `const` | [`packages/host/apiproxy/src/api-proxy.ts:2037`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/src/api-proxy.ts#L2037) | `const cancelled = () => err<{ items: SessionSearchItem[]; hasMore: boolean }>(request, {` |
| `ApprovalResponsePayload` | `interface` | [`packages/host/apiproxy/src/api/approvals.ts:17`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/src/api/approvals.ts#L17) | `export interface ApprovalResponsePayload {` |
| `RpcMethodMap` | `interface` | [`packages/host/apiproxy/src/api/rpc-map.ts:24`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/src/api/rpc-map.ts#L24) | `export interface RpcMethodMap {` |
| `sandbox` | `const` | [`packages/interaction/permission-presets/src/index.ts:310`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/interaction/permission-presets/src/index.ts#L310) | `const sandbox = state.sandbox ?? this.ctx.shell.sandboxMode` |
| `approval` | `const` | [`packages/interaction/permission-presets/src/index.ts:311`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/interaction/permission-presets/src/index.ts#L311) | `const approval = state.approval ?? this.ctx.approval.config.policy ?? 'ask'` |
| `sandbox` | `const` | [`packages/interaction/permission-presets/src/index.ts:403`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/interaction/permission-presets/src/index.ts#L403) | `const sandbox = effectiveSandboxMode(events)` |
| `approval` | `const` | [`packages/interaction/permission-presets/src/index.ts:404`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/interaction/permission-presets/src/index.ts#L404) | `const approval = effectiveApprovalPolicy(events)` |
| `PermissionSelect` | `interface` | [`packages/interaction/permission-presets/src/types.ts:27`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/interaction/permission-presets/src/types.ts#L27) | `export interface PermissionSelect {` |
| `hasOpenTurn` | `function` | [`packages/interaction/user-approval/src/index.ts:127`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/interaction/user-approval/src/index.ts#L127) | `function hasOpenTurn(events: readonly SessionEvent[]): boolean {` |

### Tests and executable evidence

- [`python/sdk/tests/test_client.py`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/python/sdk/tests/test_client.py) — A test under the owning area exercises or imports `respond`.
- [`packages/shell/bash-sandbox/tests/bwrap.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/shell/bash-sandbox/tests/bwrap.e2e.ts) — A test under the owning area exercises or imports `dsh-sandbox-local`. A test under the owning area exercises or imports `dsh-bash-sandbox`.
- [`packages/fs/fs-sandbox/tests/fs-sandbox.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/fs-sandbox/tests/fs-sandbox.spec.ts) — A test under the owning area exercises or imports `dsh-fs-sandbox`.
- [`packages/shell/bash-local/tests/settings.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/shell/bash-local/tests/settings.spec.ts) — A test under the owning area exercises or imports `dsh-bash-local`.
- [`packages/shell/bash-local/tests/executor.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/shell/bash-local/tests/executor.spec.ts) — A test under the owning area exercises or imports `dsh-bash-local`.
- [`packages/shell/bash-sandbox/tests/landlock.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/shell/bash-sandbox/tests/landlock.e2e.ts) — A test under the owning area exercises or imports `dsh-sandbox-local`. A test under the owning area exercises or imports `dsh-sandbox-policy`.
- [`packages/shell/bash-sandbox/tests/seatbelt.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/shell/bash-sandbox/tests/seatbelt.e2e.ts) — A test under the owning area exercises or imports `dsh-sandbox-local`. A test under the owning area exercises or imports `dsh-bash-sandbox`.
- [`packages/sandbox/sandbox-local/tests/bwrap.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/sandbox/sandbox-local/tests/bwrap.e2e.ts) — A test under the owning area exercises or imports `dsh-sandbox-local`.

## How to read the implementation

1. Start with [`packages/fs/fs-local/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/fs-local/src/index.ts) because it has the strongest evidence link to the note.
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

- Tags: `class/feature`, `concern/boundary`, `concern/cancellation-timeout`, `concern/concurrency`, `concern/discovery-routing`, `concern/human-control`, `concern/lifecycle`, `concern/ownership`, `concern/recovery`, `concern/schema-types`, `concern/trust`, `domain/agent-loop`, `domain/build-release`, `domain/configuration`, `domain/filesystem`, `domain/llm`, `domain/protocols`, `domain/security`, `domain/session-state`, `domain/shell-terminal`, `domain/storage`, `domain/testing`, `domain/ui-interaction`, `domain/web-retrieval`, `lifecycle/implemented`, `mechanism/event-sourcing`, `mechanism/generation`, `mechanism/policy`, `mechanism/registry`, `mechanism/streaming`
- Aliases: `PendingWait`, `composer`, `ApprovalPanel`, `permissions`, `PendingApproval`, `createApiProxy`, `decided`, `cancelled`, `ApprovalResponsePayload`, `RpcMethodMap`, `sandbox`, `approval`, `PermissionSelect`, `hasOpenTurn`
- Regex: `(?i)(PendingWait|composer|ApprovalPanel|permissions|PendingApproval|createApiProxy|decided|cancelled)`

```bash
rg -n --pcre2 "(?i)(PendingWait|composer|ApprovalPanel|permissions|PendingApproval|createApiProxy|decided|cancelled)" source-deepseek-harness
rg -l --fixed-strings "class/feature" 03-agent-context
```

## Connected notes

- **`shares-code-with`** — [0198. Web subagent catalog and human continuation](0198-web-subagent-catalog-and-human-continuation.md): Shares source implementation: `packages/core/session/src/index.ts`, `packages/core/session/src/invariant.ts`.
- **`shares-code-with`** — [0293. Minimal profiles use the bare two-tool runtime](0293-minimal-profiles-use-the-bare-two-tool-runtime.md): Shares source implementation: `packages/fs/fs-local/src/index.ts`, `packages/fs/fs-local/src/invariant.ts`.
- **`shares-code-with`** — [0063. Web input state machine, composer slots, and the slash pipeline (ui-conversation input / ui-input-trigger)](0063-web-input-state-machine-composer-slots-and-the-slash-pipeline-ui-convers.md): Shares source implementation: `packages/core/session/src/index.ts`, `packages/core/session/src/invariant.ts`.
- **`shares-code-with`** — [0265. Settlement delivery belongs to the continuation manager](0265-settlement-delivery-belongs-to-the-continuation-manager.md): Shares source implementation: `packages/core/session/src/index.ts`, `packages/core/session/src/invariant.ts`.
- **`shares-code-with`** — [0495. ACP snapshot tests --- record-once / replay-deterministic](0495-acp-snapshot-tests-record-once-replay-deterministic.md): Shares source implementation: `packages/core/session/src/index.ts`, `packages/core/session/src/invariant.ts`.
- **`shares-code-with`** — [0061. Web client Agent-scope parity model and the provisioning channel (agents/scope / blank reuse / provide)](0061-web-client-agent-scope-parity-model-and-the-provisioning-channel-agents.md): Shares source implementation: `packages/core/session/src/index.ts`, `packages/core/session/src/invariant.ts`.
- **`shares-code-with`** — [0168. Cross-session references](0168-cross-session-references.md): Shares source implementation: `packages/core/session/src/index.ts`, `packages/core/session/src/invariant.ts`.
- **`shares-code-with`** — [0518. Recallable compaction --- index checkpoints, a state checkpoint, and in-session history recall](0518-recallable-compaction-index-checkpoints-a-state-checkpoint-and-in-sessio.md): Shares source implementation: `packages/core/session/src/index.ts`, `packages/core/session/src/invariant.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0178-web-ui-permission-presets-and-approval-answering.md`.
