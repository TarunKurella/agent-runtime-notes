---
id: "dsh-note-0360"
title: "The session-row identity guard covers the preset"
status: "implemented"
class: "bug-fix"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/bug-fix/2026-08-10-session-row-identity-covers-the-preset.md"
implementation_evidence: "medium"
target_anchor: "append-only event and session store"
tags:
  - "class/bug-fix"
  - "concern/boundary"
  - "concern/discovery-routing"
  - "concern/evidence"
  - "concern/ownership"
  - "domain/configuration"
  - "domain/session-state"
  - "domain/storage"
  - "domain/testing"
  - "lifecycle/implemented"
  - "mechanism/policy"
  - "mechanism/projection"
aliases:
  - "projectionValues"
  - "applyMutation"
  - "SessionSummary"
  - "updatedAt"
  - "standard"
  - "agentPreset"
  - "SessionManager.buildListSnapshot"
  - "buildListSnapshot"
  - "SessionListItem"
  - "noteAgentPreset"
  - "SessionSummary.agentPreset"
  - "sessions-service"
  - "sessions-service.spec.ts"
  - "agent-preset-selection"
search_regex: "(?i)(projectionValues|applyMutation|SessionSummary|updatedAt|standard|agentPreset|SessionManager\\.buildListSnapshot|buildListSnapshot)"
---

# 0360. The session-row identity guard covers the preset — implementation context

## Open this when

SessionManager.buildListSnapshot memoizes list rows by value: a wire refresh mints all-new summary objects, so an entry equal to the cached one is replaced by the cached instance, and every SessionListItem memo downstream keeps hitting. The stated contract is "reuse the cached object when every field matches"; the comparison enumerated the fields by hand and did not enumerate agentPreset. A confirmed preset switch moves exactly that one field.

## Source decision

The identity guard compares agentPreset alongside the other summary fields, which is what "every field matches" already claimed. Nothing else changes: the memoization, the merge, and the chip's no-op check all stay as they are, because each is correct once the row it reads is.

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/bug-fix/2026-08-10-session-row-identity-covers-the-preset.md](../02-notes/implemented/bug-fix/2026-08-10-session-row-identity-covers-the-preset.md)
- Pinned source: [.agents/notes/implemented/bug-fix/2026-08-10-session-row-identity-covers-the-preset.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/bug-fix/2026-08-10-session-row-identity-covers-the-preset.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`packages/goal/goal/src/fold.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/goal/goal/src/fold.ts) | runtime implementation | Defines `updatedAt`, a construct named by the note. | `symbol-definition` |
| [`packages/goal/goal/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/goal/goal/src/index.ts) | package entry point | Defines `updatedAt`, a construct named by the note. | `symbol-definition` |
| [`packages/host/apiproxy/src/api-proxy.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/src/api-proxy.ts) | runtime implementation | Defines `agentPreset`, a construct named by the note. | `symbol-definition` |
| [`packages/host/apiproxy/src/api/sessions.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/src/api/sessions.ts) | runtime implementation | Defines `SessionSummary`, a construct named by the note. | `symbol-definition` |
| [`packages/subagent/subagent/src/child-agent.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/subagent/subagent/src/child-agent.ts) | runtime implementation | Defines `agentPreset`, a construct named by the note. | `symbol-definition` |
| [`packages/client/web-react/src/scoped-slots.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/web-react/src/scoped-slots.tsx) | runtime implementation | Defines `standard`, a construct named by the note. | `symbol-definition` |
| [`packages/client/runtime/src/client/sessions/manager.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/runtime/src/client/sessions/manager.ts) | runtime implementation | Defines `applyMutation`, a construct named by the note. Defines `projectionValues`, a construct named by the note. | `symbol-definition` |
| [`packages/client/runtime/src/client/sessions/service.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/runtime/src/client/sessions/service.ts) | runtime implementation | Defines `SessionSummary`, a construct named by the note. | `symbol-definition` |
| [`packages/client/ui-workspace/src/client/WorkspaceBrowser.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-workspace/src/client/WorkspaceBrowser.tsx) | runtime implementation | Defines `updatedAt`, a construct named by the note. | `symbol-definition` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `projectionValues` | `const` | [`packages/client/runtime/src/client/sessions/manager.ts:1026`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/runtime/src/client/sessions/manager.ts#L1026) | `const projectionValues = projectionStore?.values()` |
| `applyMutation` | `function` | [`packages/client/runtime/src/client/sessions/manager.ts:1080`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/runtime/src/client/sessions/manager.ts#L1080) | `function applyMutation(summaries: readonly SessionSummary[], mutation: SessionListMutation): SessionSummary[] {` |
| `SessionSummary` | `interface` | [`packages/client/runtime/src/client/sessions/service.ts:42`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/runtime/src/client/sessions/service.ts#L42) | `export interface SessionSummary {` |
| `updatedAt` | `const` | [`packages/client/ui-workspace/src/client/WorkspaceBrowser.tsx:133`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-workspace/src/client/WorkspaceBrowser.tsx#L133) | `const updatedAt: Record<string, number> = {}` |
| `standard` | `let` | [`packages/client/web-react/src/scoped-slots.tsx:364`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/web-react/src/scoped-slots.tsx#L364) | `let standard = byInfo.get(info)` |
| `standard` | `const` | [`packages/client/web-react/src/scoped-slots.tsx:405`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/web-react/src/scoped-slots.tsx#L405) | `const standard = standardProps(host, scope, info)` |
| `updatedAt` | `const` | [`packages/goal/goal/src/fold.ts:161`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/goal/goal/src/fold.ts#L161) | `const updatedAt = nonNegativeInteger(value['updatedAt'], 'updatedAt')` |
| `updatedAt` | `const` | [`packages/goal/goal/src/index.ts:508`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/goal/goal/src/index.ts#L508) | `const updatedAt = cache.state.updatedAt` |
| `updatedAt` | `const` | [`packages/goal/goal/src/index.ts:564`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/goal/goal/src/index.ts#L564) | `const updatedAt = cache.state.updatedAt` |
| `agentPreset` | `const` | [`packages/host/apiproxy/src/api-proxy.ts:544`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/src/api-proxy.ts#L544) | `const agentPreset = resolveSessionPreset({ header, events })` |
| `SessionSummary` | `interface` | [`packages/host/apiproxy/src/api/sessions.ts:177`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/src/api/sessions.ts#L177) | `export interface SessionSummary {` |
| `agentPreset` | `const` | [`packages/subagent/subagent/src/child-agent.ts:108`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/subagent/subagent/src/child-agent.ts#L108) | `const agentPreset = parent.ctx.get('agentPresets')?.composedPreset(parent.ctx)` |

### Tests and executable evidence

- [`apps/cli/tests/built-bin.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/cli/tests/built-bin.e2e.ts) — A test under the owning area exercises or imports `minimal`.
- [`apps/web/tests/assembled-boot.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/assembled-boot.ts) — A test under the owning area exercises or imports `minimal`.
- [`apps/web/tests/seeded-history.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/seeded-history.e2e.ts) — A test under the owning area exercises or imports `minimal`.
- [`apps/cli/tests/windows-shell.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/cli/tests/windows-shell.spec.ts) — A test under the owning area exercises or imports `minimal`.
- [`apps/web/tests/built-boot.snapshot.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/built-boot.snapshot.ts) — A test under the owning area exercises or imports `minimal`.
- [`apps/cli/tests/web-agent-presets.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/cli/tests/web-agent-presets.e2e.ts) — A test under the owning area exercises or imports `minimal`.
- [`apps/web/tests/declared-reasoning.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/declared-reasoning.e2e.ts) — A test under the owning area exercises or imports `minimal`.
- [`python/sdk/tests/test_bundled_runtime.py`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/python/sdk/tests/test_bundled_runtime.py) — A test under the owning area exercises or imports `minimal`.
- Source verification intent: sessions-service.spec.ts feeds a blank row, notes a switch, and asserts the projected snapshot reports the new preset --- it fails on the old guard because the row differs in nothing else. The agent-preset-selection web e2e switches down and back up, asserting the host honors the second switch and the / catalog returns with it; without this fix the second switch never reaches the host at all.

## How to read the implementation

1. Start with [`packages/goal/goal/src/fold.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/goal/goal/src/fold.ts) because it has the strongest evidence link to the note.
2. Read public types and entry points before implementation details. Write down the contract in plain language.
3. Follow the named constructs into their callers and tests. Confirm which component can actually enforce the rule.
4. Reconstruct one failure path: cancellation, crash, retry, stale state, partial write, or incompatible provider input—whichever the note is protecting against.
5. Only then map the contract into the current Rust repository. Target paths must come from that repository, never from guesswork in this wiki.

## Rust harness port

**Likely integration point:** append-only event and session store.

Preserve the behavior and ownership rule. Do not mechanically translate Cordis, TypeScript declaration merging, Node streams, or package topology into Rust.

Relevant standing rules:

- **`rust/append-log`** — Keep durable facts append-only and derive views from them. Separate the synchronous fact boundary from asynchronous persistence.
- **`rust/four-tool-surface`** — Preserve the four model tools: read, write, patch, and exec. Reuse internal capabilities without automatically exposing new model-facing tools.
- **`rust/quickjs-boundary`** — QuickJS holds orchestration variables; Rust/Tokio owns async execution and exposes only explicitly granted capabilities.
- **`rust/trait-seam`** — Use a small trait at the capability boundary; keep policy and model-facing schemas outside the backend implementation.

### Smallest safe implementation slice

1. Express the semantic contract as Rust types and one owning boundary.
2. Add the smallest adapter or service needed by the existing loop; do not add a new model-facing tool unless the four-tool surface cannot express the job.
3. Record durable evidence before deriving summaries or UI state.
4. Add one happy-path test, one interruption test, and one replay/restart test where state survives a turn.
5. Keep the patch reversible until the source note's strongest failure case is proven in the Rust harness.

## Search handles

- Tags: `class/bug-fix`, `concern/boundary`, `concern/discovery-routing`, `concern/evidence`, `concern/ownership`, `domain/configuration`, `domain/session-state`, `domain/storage`, `domain/testing`, `lifecycle/implemented`, `mechanism/policy`, `mechanism/projection`
- Aliases: `projectionValues`, `applyMutation`, `SessionSummary`, `updatedAt`, `standard`, `agentPreset`, `SessionManager.buildListSnapshot`, `buildListSnapshot`, `SessionListItem`, `noteAgentPreset`, `SessionSummary.agentPreset`, `sessions-service`, `sessions-service.spec.ts`, `agent-preset-selection`
- Regex: `(?i)(projectionValues|applyMutation|SessionSummary|updatedAt|standard|agentPreset|SessionManager\.buildListSnapshot|buildListSnapshot)`

```bash
rg -n --pcre2 "(?i)(projectionValues|applyMutation|SessionSummary|updatedAt|standard|agentPreset|SessionManager\\.buildListSnapshot|buildListSnapshot)" source-deepseek-harness
rg -l --fixed-strings "class/bug-fix" 03-agent-context
```

## Connected notes

- **`source-link`** — [0361. The slash catalog follows a blank session's preset switch](0361-the-slash-catalog-follows-a-blank-session-s-preset-switch.md): The source note links to this decision directly.
- **`shares-code-with`** — [0183. Session List Browsing and Manual Workspace Order](0183-session-list-browsing-and-manual-workspace-order.md): Shares source implementation: `packages/goal/goal/src/fold.ts`, `packages/goal/goal/src/index.ts`.
- **`shares-code-with`** — [0472. One route to add a Workspace](0472-one-route-to-add-a-workspace.md): Shares source implementation: `packages/client/ui-workspace/src/client/WorkspaceBrowser.tsx`, `packages/goal/goal/src/fold.ts`.
- **`shares-code-with`** — [0199. Workspace Registration Deletion](0199-workspace-registration-deletion.md): Shares source implementation: `packages/client/web-react/src/scoped-slots.tsx`, `packages/host/apiproxy/src/api-proxy.ts`.
- **`shares-code-with`** — [0375. Bound cold blank-session verification](0375-bound-cold-blank-session-verification.md): Shares source implementation: `packages/goal/goal/src/fold.ts`, `packages/host/apiproxy/src/api-proxy.ts`.
- **`shares-code-with`** — [0438. Event-directed PR review status commands](0438-event-directed-pr-review-status-commands.md): Shares source implementation: `packages/goal/goal/src/fold.ts`, `packages/goal/goal/src/index.ts`.
- **`shares-code-with`** — [0513. Record last activity in the session index](0513-record-last-activity-in-the-session-index.md): Shares source implementation: `packages/goal/goal/src/fold.ts`, `packages/host/apiproxy/src/api-proxy.ts`.
- **`shares-code-with`** — [0185. Workspace UI Complete Product Flow](0185-workspace-ui-complete-product-flow.md): Shares source implementation: `packages/goal/goal/src/fold.ts`, `packages/host/apiproxy/src/api-proxy.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0360-the-session-row-identity-guard-covers-the-preset.md`.
