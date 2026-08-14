---
id: "dsh-note-0509"
title: "Domain KV storage capability seam and the workspace entity"
status: "proposed"
class: "architecture"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/proposed/architecture/2026-07-24-domain-kv-storage-and-workspace.md"
implementation_evidence: "high"
target_anchor: "sandbox capability boundary"
tags:
  - "class/architecture"
  - "concern/boundary"
  - "concern/cancellation-timeout"
  - "concern/compatibility"
  - "concern/concurrency"
  - "concern/discovery-routing"
  - "concern/evidence"
  - "concern/human-control"
  - "concern/lifecycle"
  - "concern/ownership"
  - "concern/performance"
  - "concern/recovery"
  - "concern/schema-types"
  - "concern/simplification"
  - "concern/trust"
  - "domain/build-release"
  - "domain/configuration"
  - "domain/context"
  - "domain/extensions"
  - "domain/filesystem"
  - "domain/llm"
  - "domain/observability"
  - "domain/protocols"
  - "domain/security"
  - "domain/session-state"
  - "domain/shell-terminal"
  - "domain/storage"
  - "domain/testing"
  - "domain/ui-interaction"
  - "lifecycle/proposed"
  - "mechanism/event-sourcing"
  - "mechanism/generation"
  - "mechanism/projection"
  - "mechanism/registry"
  - "mechanism/streaming"
aliases:
  - "closed"
  - "json"
  - "mutate"
  - "path"
  - "storage"
  - "updatedAt"
  - "list"
  - "routes"
  - "mount"
  - "SessionPersistence"
  - "get"
  - "Domain"
  - "DomainError"
  - "DomainChanged"
search_regex: "(?i)(closed|json|mutate|path|storage|updatedAt|list|routes)"
---

# 0509. Domain KV storage capability seam and the workspace entity — implementation context

## Open this when

The host's only persistence surface is the session event log (packages/session/session-persistence: append-only, one file per session). Anything that does not belong to a single session has nowhere to live, and two real needs exist today: The workspace entity. The GUI needs workspace as a real object: path, title, and the list of owned sessions. Ownership belongs to the workspace --- "which sessions belong to this workspace" is not any single session's fact, so writing it into the session log is semantically wrong. Before this design, workspace was only a sidebar visual grouping derived from cwd, with no entity.

## Source decision

Create the packages/storage/ group --- the ctx.storage hub (backend registry + data-form mounts), two backends, the domain data form --- plus the workspace consumer package; extend SessionPersistence with a delete primitive. (workspace lives in its own group rather than packages/host/: the host group's naming rule requires the dsh-host- prefix while this package is named dsh-workspace; and the workspace entity is a domain concept, not bound to the host assembly tier.

## Decision status

Proposal, not proof of shipped behavior. Reuse the pressure and acceptance criteria; verify whether any candidate code is partial scaffolding before porting it.

- Raw note: [proposed/architecture/2026-07-24-domain-kv-storage-and-workspace.md](../02-notes/proposed/architecture/2026-07-24-domain-kv-storage-and-workspace.md)
- Pinned source: [.agents/notes/proposed/architecture/2026-07-24-domain-kv-storage-and-workspace.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/proposed/architecture/2026-07-24-domain-kv-storage-and-workspace.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`packages/storage/storage/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/storage/storage/src/index.ts) | package entry point | Entry point or contract under the directory named by the note: `packages/storage`. Entry point or contract under the directory named by the note: `packages/storage/storage`. | `named-directory-member, named-file, named-package-member, symbol-definition` |
| [`packages/storage/storage/src/error.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/storage/storage/src/error.ts) | runtime implementation | Entry point or contract under the directory named by the note: `packages/storage/storage`. The source note names this file directly. | `named-directory-member, named-file, named-package-member, symbol-definition` |
| [`packages/storage/storage/src/backend.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/storage/storage/src/backend.ts) | provider/backend adapter | The source note names this file directly. | `named-file` |
| [`packages/storage/storage-domain/src/events.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/storage/storage-domain/src/events.ts) | runtime implementation | Entry point or contract under the directory named by the note: `packages/storage/storage-domain`. The source note names this file directly. | `named-directory-member, named-file, named-package-member, symbol-definition` |
| [`packages/storage/storage/src/registry.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/storage/storage/src/registry.ts) | runtime implementation | Entry point or contract under the directory named by the note: `packages/storage`. Entry point or contract under the directory named by the note: `packages/storage/storage`. | `named-directory-member, named-package-member, symbol-definition` |
| [`packages/storage/storage/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/storage/storage/src/invariant.ts) | runtime contract checks | Entry point or contract under the directory named by the note: `packages/storage/storage`. Core file in the package named by the note: `packages/storage/storage`. | `named-directory-member, named-package-member` |
| [`packages/workspace/workspace/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/workspace/workspace/src/index.ts) | package entry point | Entry point or contract under the directory named by the note: `packages/workspace/workspace`. Core file in the package named by the note: `packages/workspace/workspace`. | `named-directory-member, named-package-member, symbol-definition` |
| [`packages/workspace/workspace/src/types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/workspace/workspace/src/types.ts) | public types and contract | Entry point or contract under the directory named by the note: `packages/workspace/workspace`. Core file in the package named by the note: `packages/workspace/workspace`. | `named-directory-member, named-package-member, symbol-definition` |
| [`packages/storage/storage-json/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/storage/storage-json/src/index.ts) | package entry point | Entry point or contract under the directory named by the note: `packages/storage`. Entry point or contract under the directory named by the note: `packages/storage/storage-json`. | `named-directory-member, named-package-member, symbol-definition` |
| [`packages/workspace/workspace/src/entity.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/workspace/workspace/src/entity.ts) | runtime implementation | Entry point or contract under the directory named by the note: `packages/workspace/workspace`. Core file in the package named by the note: `packages/workspace/workspace`. | `named-directory-member, named-package-member, symbol-definition` |
| [`packages/storage/storage-json/src/format.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/storage/storage-json/src/format.ts) | runtime implementation | Entry point or contract under the directory named by the note: `packages/storage`. Entry point or contract under the directory named by the note: `packages/storage/storage-json`. | `named-directory-member, named-package-member` |
| [`packages/storage/storage-domain/src/error.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/storage/storage-domain/src/error.ts) | runtime implementation | Entry point or contract under the directory named by the note: `packages/storage`. Entry point or contract under the directory named by the note: `packages/storage/storage-domain`. | `named-directory-member, named-package-member, symbol-definition` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `closed` | `let` | [`packages/acp/acp/src/index.ts:111`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/acp/acp/src/index.ts#L111) | `let closed = false` |
| `json` | `const` | [`packages/client/modules/src/index.ts:169`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/modules/src/index.ts#L169) | `const json = JSON.stringify(graph).replaceAll('<', '\\u003c')` |
| `mutate` | `const` | [`packages/client/runtime/src/client/contract/store.ts:220`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/runtime/src/client/contract/store.ts#L220) | `const mutate = decl.actions[key] as (draft: T, ...params: unknown[]) => void` |
| `path` | `const` | [`packages/context/agent-instructions/src/files.ts:248`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/context/agent-instructions/src/files.ts#L248) | `const path = join(dir, candidate)` |
| `storage` | `const` | [`packages/e2b/fs-e2b/src/index.ts:425`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/e2b/fs-e2b/src/index.ts#L425) | `const storage = restoreLineEndings(after, detectsCrlf(raw))` |
| `updatedAt` | `const` | [`packages/goal/goal/src/fold.ts:161`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/goal/goal/src/fold.ts#L161) | `const updatedAt = nonNegativeInteger(value['updatedAt'], 'updatedAt')` |
| `list` | `const` | [`packages/hooks/hook-protocol/src/merge.ts:75`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/hooks/hook-protocol/src/merge.ts#L75) | `const list = reasonsByRank.get(r) ?? []` |
| `routes` | `const` | [`packages/llm/llm-pi-ai/src/index.ts:262`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm-pi-ai/src/index.ts#L262) | `const routes = [...profiles().keys()]` |
| `mount` | `const` | [`packages/preset/agent-presets/src/mount.ts:261`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/preset/agent-presets/src/mount.ts#L261) | `const mount = standingMountFor(agent.ctx)` |
| `SessionPersistence` | `class` | [`packages/session/session-persistence/src/index.ts:84`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/session/session-persistence/src/index.ts#L84) | `export abstract class SessionPersistence extends Service {` |
| `get` | `const` | [`packages/shell/tool-bash-persistent/src/index.ts:227`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/shell/tool-bash-persistent/src/index.ts#L227) | `const get = (owner: Agent, signal: AbortSignal): Promise<TerminalSessionId> => {` |
| `Domain` | `interface` | [`packages/storage/storage-domain/src/domain.ts:97`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/storage/storage-domain/src/domain.ts#L97) | `export interface Domain<S extends DomainSpec> {` |
| `DomainError` | `class` | [`packages/storage/storage-domain/src/error.ts:34`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/storage/storage-domain/src/error.ts#L34) | `export class DomainError extends Error {` |
| `DomainChanged` | `type` | [`packages/storage/storage-domain/src/events.ts:34`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/storage/storage-domain/src/events.ts#L34) | `export type DomainChanged = DomainChangedPut \| DomainChangedDeleted` |
| `StorageForms` | `interface` | [`packages/storage/storage-domain/src/index.ts:30`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/storage/storage-domain/src/index.ts#L30) | `interface StorageForms {` |
| `backend` | `const` | [`packages/storage/storage-domain/src/index.ts:107`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/storage/storage-domain/src/index.ts#L107) | `const backend = this.ctx.storage.backend.get(backendName)` |

### Tests and executable evidence

- [`packages/typert/generator/tests/fixtures/remote-model/packages/domain/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/typert/generator/tests/fixtures/remote-model/packages/domain/src/index.ts) — Core file in the package named by the note: `packages/typert/generator/tests/fixtures/remote-model/packages/domain`.
- [`packages/typert/generator/tests/fixtures/remote-model/packages/domain/src/types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/typert/generator/tests/fixtures/remote-model/packages/domain/src/types.ts) — Core file in the package named by the note: `packages/typert/generator/tests/fixtures/remote-model/packages/domain`.
- [`packages/typert/generator/tests/fixtures/type-model`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/typert/generator/tests/fixtures/type-model) — The note names this package or capability.
- [`packages/typert/generator/tests/fixtures/remote-model/packages/domain`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/typert/generator/tests/fixtures/remote-model/packages/domain) — The note names this package or capability.
- [`packages/typert/generator/tests/fixtures/type-model/package.json`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/typert/generator/tests/fixtures/type-model/package.json) — Core file in the package named by the note: `packages/typert/generator/tests/fixtures/type-model`.
- [`packages/typert/generator/tests/fixtures/remote-model/packages/domain/package.json`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/typert/generator/tests/fixtures/remote-model/packages/domain/package.json) — Core file in the package named by the note: `packages/typert/generator/tests/fixtures/remote-model/packages/domain`.
- [`packages/storage/storage/tests/contract.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/storage/storage/tests/contract.ts) — A test under the owning area exercises or imports `sqlite`. A test under the owning area exercises or imports `loadAll`.
- [`packages/storage/storage/tests/registry.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/storage/storage/tests/registry.spec.ts) — A test under the owning area exercises or imports `Storage`. A test under the owning area exercises or imports `BackendRegistry`.
- Source verification intent: This phase's four test suites all green: the shared backend contract suite on both json/sqlite, registry/mount disposer semantics, the domain layer (including the six open steps and fail-loud routing), and full workspace semantics (create/attach checks/consistency doctrine). ctx.workspaceRegistry completes the create → attach → list → metadata-only delete lifecycle under a test assembly. Zero diff in the session-persistence packages (the acceptance line for not touching the session side this phase). No new snapshots this phase (no model-visible or assembly surface); added next phase with the RPC wiring.

## How to read the implementation

1. Start with [`packages/storage/storage/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/storage/storage/src/index.ts) because it has the strongest evidence link to the note.
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
- **`rust/process-discipline`** — Treat this as repository or release policy unless it changes a runtime contract. Do not add runtime machinery for a process-only lesson.
- **`rust/provider-adapter`** — Keep provider wire quirks inside adapters; the loop and durable event vocabulary remain provider-neutral.
- **`rust/quickjs-boundary`** — QuickJS holds orchestration variables; Rust/Tokio owns async execution and exposes only explicitly granted capabilities.
- **`rust/serde-validation`** — Parse untrusted input into strict serde types and validate semantic constraints before executing side effects.
- **`rust/tagged-enums`** — Use serde-tagged enums for a closed wire vocabulary. Add an explicit extension envelope only where runtime extension is truly required.
- **`rust/tokio-cancellation`** — Thread one cancellation token through async work, close owned resources before reuse, and test cancellation at every await boundary.
- **`rust/trait-seam`** — Use a small trait at the capability boundary; keep policy and model-facing schemas outside the backend implementation.
- **Status gate** — Proposal, not proof of shipped behavior. Reuse the pressure and acceptance criteria; verify whether any candidate code is partial scaffolding before porting it.

### Smallest safe implementation slice

1. Express the semantic contract as Rust types and one owning boundary.
2. Add the smallest adapter or service needed by the existing loop; do not add a new model-facing tool unless the four-tool surface cannot express the job.
3. Record durable evidence before deriving summaries or UI state.
4. Add one happy-path test, one interruption test, and one replay/restart test where state survives a turn.
5. Keep the patch reversible until the source note's strongest failure case is proven in the Rust harness.

## Search handles

- Tags: `class/architecture`, `concern/boundary`, `concern/cancellation-timeout`, `concern/compatibility`, `concern/concurrency`, `concern/discovery-routing`, `concern/evidence`, `concern/human-control`, `concern/lifecycle`, `concern/ownership`, `concern/performance`, `concern/recovery`, `concern/schema-types`, `concern/simplification`, `concern/trust`, `domain/build-release`, `domain/configuration`, `domain/context`, `domain/extensions`, `domain/filesystem`, `domain/llm`, `domain/observability`, `domain/protocols`, `domain/security`, `domain/session-state`, `domain/shell-terminal`, `domain/storage`, `domain/testing`, `domain/ui-interaction`, `lifecycle/proposed`, `mechanism/event-sourcing`, `mechanism/generation`, `mechanism/projection`, `mechanism/registry`, `mechanism/streaming`
- Aliases: `closed`, `json`, `mutate`, `path`, `storage`, `updatedAt`, `list`, `routes`, `mount`, `SessionPersistence`, `get`, `Domain`, `DomainError`, `DomainChanged`
- Regex: `(?i)(closed|json|mutate|path|storage|updatedAt|list|routes)`

```bash
rg -n --pcre2 "(?i)(closed|json|mutate|path|storage|updatedAt|list|routes)" source-deepseek-harness
rg -l --fixed-strings "class/architecture" 03-agent-context
```

## Connected notes

- **`source-link`** — [0199. Workspace Registration Deletion](0199-workspace-registration-deletion.md): The source note links to this decision directly.
- **`same-design-pressure`** — [0040. LSP capability seam and model-facing query tool](0040-lsp-capability-seam-and-model-facing-query-tool.md): Shares design concerns: `concern/boundary`, `concern/cancellation-timeout`, `concern/compatibility`.
- **`same-design-pressure`** — [0146. The subprocess sandbox --- confinement seam, native runners, escalation, and per-session modes](0146-the-subprocess-sandbox-confinement-seam-native-runners-escalation-and-pe.md): Shares design concerns: `concern/boundary`, `concern/compatibility`, `concern/concurrency`.
- **`same-design-pressure`** — [0500. Keyless browser e2e lane for the web GUI](0500-keyless-browser-e2e-lane-for-the-web-gui.md): Shares design concerns: `concern/boundary`, `concern/cancellation-timeout`, `concern/compatibility`.
- **`same-design-pressure`** — [0557. Agent Client Protocol (ACP) support --- drive the coding agent from external editors](0557-agent-client-protocol-acp-support-drive-the-coding-agent-from-external-e.md): Shares design concerns: `concern/boundary`, `concern/cancellation-timeout`, `concern/compatibility`.
- **`shares-code-with`** — [0183. Session List Browsing and Manual Workspace Order](0183-session-list-browsing-and-manual-workspace-order.md): Shares source implementation: `packages/workspace/workspace/src/entity.ts`, `packages/workspace/workspace/src/index.ts`.
- **`shares-code-with`** — [0383. Subsystems catalog and the `ts type-equiv` drift gate](0383-subsystems-catalog-and-the-ts-type-equiv-drift-gate.md): Shares source implementation: `packages/storage/storage/src/index.ts`, `packages/storage/storage/src/invariant.ts`.
- **`same-design-pressure`** — [0542. Dependency swaps rejected by the 2026-07 NIH audit](0542-dependency-swaps-rejected-by-the-2026-07-nih-audit.md): Shares design concerns: `concern/boundary`, `concern/cancellation-timeout`, `concern/compatibility`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0509-domain-kv-storage-capability-seam-and-the-workspace-entity.md`.
