---
id: "dsh-note-0191"
title: "Native workspace directory picker"
status: "implemented"
class: "feature"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/feature/2026-07-27-native-workspace-directory-picker.md"
implementation_evidence: "medium"
target_anchor: "read/write/patch filesystem capability"
tags:
  - "class/feature"
  - "concern/boundary"
  - "concern/cancellation-timeout"
  - "concern/discovery-routing"
  - "concern/evidence"
  - "concern/human-control"
  - "concern/recovery"
  - "concern/schema-types"
  - "concern/trust"
  - "domain/filesystem"
  - "domain/llm"
  - "domain/session-state"
  - "domain/shell-terminal"
  - "domain/ui-interaction"
  - "domain/web-retrieval"
  - "lifecycle/implemented"
aliases:
  - "WorkspaceRuntime"
  - "host.pickDirectory"
  - "pickDirectory"
  - "workspace.create"
  - "IFileOpenDialog"
  - "Native workspace directory picker"
  - "feature"
  - "boundary"
  - "cancellation timeout"
  - "discovery routing"
  - "evidence"
  - "human control"
  - "recovery"
  - "schema types"
search_regex: "(?i)(WorkspaceRuntime|host\\.pickDirectory|pickDirectory|workspace\\.create|IFileOpenDialog|Native[- ]workspace[- ]directory[- ]picker|feature|boundary)"
---

# 0191. Native workspace directory picker — implementation context

## Open this when

The desktop GUI asks users to type an absolute path when they add an existing workspace. This is slower and more error-prone than choosing a directory with the operating system's native picker. The GUI is delivered through the local Web carrier, so opening a native dialog also creates a privileged boundary that ordinary remote requests must not cross.

## Source decision

Add a single-folder host.pickDirectory RPC and expose it through WorkspaceRuntime. The workspace menu presents the flat Add workspace... action (two actions when this was decided --- Open local folder... beside a create-by-name entry the one-route Note later removed). Selecting a folder reuses the existing workspace.create({ path }) flow, selects the returned workspace, and starts a blank session. The workspace manager must upsert the returned workspace before the selection callback runs. A newly adopted directory therefore renders its basename immediately.

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/feature/2026-07-27-native-workspace-directory-picker.md](../02-notes/implemented/feature/2026-07-27-native-workspace-directory-picker.md)
- Pinned source: [.agents/notes/implemented/feature/2026-07-27-native-workspace-directory-picker.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/feature/2026-07-27-native-workspace-directory-picker.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`packages/client/runtime/src/client/workspaces/service.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/runtime/src/client/workspaces/service.ts) | runtime implementation | Defines `WorkspaceRuntime`, a construct named by the note. | `symbol-definition` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `WorkspaceRuntime` | `class` | [`packages/client/runtime/src/client/workspaces/service.ts:51`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/runtime/src/client/workspaces/service.ts#L51) | `export class WorkspaceRuntime implements IWorkspaces {` |

### Tests and executable evidence

- [`packages/client/runtime/tests/fake-api.client.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/runtime/tests/fake-api.client.ts) — A test under the owning area exercises or imports `pickDirectory`.
- [`packages/host/apiproxy/tests/fetch-carrier.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/tests/fetch-carrier.spec.ts) — A test under the owning area exercises or imports `pickDirectory`.
- [`packages/client/connection/tests/fake-api.client.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/connection/tests/fake-api.client.ts) — A test under the owning area exercises or imports `pickDirectory`.
- [`packages/host/apiproxy/tests/client-handler.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/tests/client-handler.spec.ts) — A test under the owning area exercises or imports `pickDirectory`.
- [`packages/client/connection/tests/node-half.host.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/connection/tests/node-half.host.spec.ts) — A test under the owning area exercises or imports `pickDirectory`.
- [`packages/host/apiproxy/tests/api-proxy-workspace.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/tests/api-proxy-workspace.spec.ts) — A test under the owning area exercises or imports `pickDirectory`.
- [`packages/client/connection/tests/http-bridge.host.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/connection/tests/http-bridge.host.spec.ts) — A test under the owning area exercises or imports `pickDirectory`.
- [`packages/client/runtime/tests/client-apply.client.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/runtime/tests/client-apply.client.spec.ts) — A test under the owning area exercises or imports `WorkspaceRuntime`.

## How to read the implementation

1. Start with [`packages/client/runtime/src/client/workspaces/service.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/runtime/src/client/workspaces/service.ts) because it has the strongest evidence link to the note.
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

- Tags: `class/feature`, `concern/boundary`, `concern/cancellation-timeout`, `concern/discovery-routing`, `concern/evidence`, `concern/human-control`, `concern/recovery`, `concern/schema-types`, `concern/trust`, `domain/filesystem`, `domain/llm`, `domain/session-state`, `domain/shell-terminal`, `domain/ui-interaction`, `domain/web-retrieval`, `lifecycle/implemented`
- Aliases: `WorkspaceRuntime`, `host.pickDirectory`, `pickDirectory`, `workspace.create`, `IFileOpenDialog`, `Native workspace directory picker`, `feature`, `boundary`, `cancellation timeout`, `discovery routing`, `evidence`, `human control`, `recovery`, `schema types`
- Regex: `(?i)(WorkspaceRuntime|host\.pickDirectory|pickDirectory|workspace\.create|IFileOpenDialog|Native[- ]workspace[- ]directory[- ]picker|feature|boundary)`

```bash
rg -n --pcre2 "(?i)(WorkspaceRuntime|host\\.pickDirectory|pickDirectory|workspace\\.create|IFileOpenDialog|Native[- ]workspace[- ]directory[- ]picker|feature|boundary)" source-deepseek-harness
rg -l --fixed-strings "class/feature" 03-agent-context
```

## Connected notes

- **`source-link`** — [0332. Same-basename Workspace adoption](0332-same-basename-workspace-adoption.md): The source note links to this decision directly.
- **`source-link`** — [0245. Win32 folder picker moves to koffi in a child process](0245-win32-folder-picker-moves-to-koffi-in-a-child-process.md): The source note links to this decision directly.
- **`source-link`** — [0472. One route to add a Workspace](0472-one-route-to-add-a-workspace.md): The source note links to this decision directly.
- **`source-link`** — [0474. Drop the Windows PowerShell picker fallback](0474-drop-the-windows-powershell-picker-fallback.md): The source note links to this decision directly.
- **`shares-code-with`** — [0315. Atomic Web image admission](0315-atomic-web-image-admission.md): Shares source implementation: `packages/client/connection/tests/fake-api.client.ts`, `packages/client/runtime/tests/fake-api.client.ts`.
- **`shares-code-with`** — [0206. Tool-call file open in OS](0206-tool-call-file-open-in-os.md): Shares source implementation: `packages/host/apiproxy/tests/api-proxy-workspace.spec.ts`, `packages/host/apiproxy/tests/client-handler.spec.ts`.
- **`shares-code-with`** — [0398. GUI testing system --- the three-tier structure](0398-gui-testing-system-the-three-tier-structure.md): Shares source implementation: `packages/host/apiproxy/tests/client-handler.spec.ts`.
- **`same-design-pressure`** — [0509. Domain KV storage capability seam and the workspace entity](0509-domain-kv-storage-capability-seam-and-the-workspace-entity.md): Shares design concerns: `concern/boundary`, `concern/cancellation-timeout`, `concern/discovery-routing`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0191-native-workspace-directory-picker.md`.
