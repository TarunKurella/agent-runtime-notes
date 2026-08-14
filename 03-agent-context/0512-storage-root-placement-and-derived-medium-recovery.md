---
id: "dsh-note-0512"
title: "Storage root placement and derived-medium recovery"
status: "proposed"
class: "architecture"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/proposed/architecture/2026-07-28-storage-root-and-derived-medium-recovery.md"
implementation_evidence: "high"
target_anchor: "read/write/patch filesystem capability"
tags:
  - "class/architecture"
  - "concern/boundary"
  - "concern/compatibility"
  - "concern/discovery-routing"
  - "concern/evidence"
  - "concern/lifecycle"
  - "concern/ownership"
  - "concern/performance"
  - "concern/recovery"
  - "concern/schema-types"
  - "concern/simplification"
  - "domain/build-release"
  - "domain/configuration"
  - "domain/extensions"
  - "domain/filesystem"
  - "domain/llm"
  - "domain/session-state"
  - "domain/storage"
  - "domain/testing"
  - "lifecycle/proposed"
  - "mechanism/event-sourcing"
  - "mechanism/policy"
  - "mechanism/projection"
  - "mechanism/registry"
aliases:
  - "json"
  - "dsh"
  - "version"
  - "recovery"
  - "reject"
  - "workspace"
  - "reset"
  - "DomainError"
  - "DomainSpec"
  - "JsonStorageBackend"
  - "openJsonUnit"
  - "KvFacet"
  - "StorageError"
  - "dshHomePath"
search_regex: "(?i)(json|version|recovery|reject|workspace|reset|DomainError|DomainSpec)"
---

# 0512. Storage root placement and derived-medium recovery — implementation context

## Open this when

The persisted projection cache (note, shipped as dsh-session-projection-cache) surfaced two gaps in the storage substrate it landed on. Both are properties of the domain-KV stack (design), not of the cache itself, and both bite the cache first because it is the first derived medium on that stack. Where the files actually live (root mismatch closed; resolve-once residual still open).

## Source decision

Two independent changes, one per gap. Shipped: the Web overlay anchors storage-json.root to $DSH_HOME/storages directly in the row through the app-boot-provided dshHomePath('storages') (~/.dsh/storages by default, beside ~/.dsh/sessions; no leading dot --- the home is already a hidden tree). The helper delegates to the canonical dsh-home-paths resolver, and the session root uses the same function without duplicating its fallback and tilde rules.

## Decision status

Proposal, not proof of shipped behavior. Reuse the pressure and acceptance criteria; verify whether any candidate code is partial scaffolding before porting it.

- Raw note: [proposed/architecture/2026-07-28-storage-root-and-derived-medium-recovery.md](../02-notes/proposed/architecture/2026-07-28-storage-root-and-derived-medium-recovery.md)
- Pinned source: [.agents/notes/proposed/architecture/2026-07-28-storage-root-and-derived-medium-recovery.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/proposed/architecture/2026-07-28-storage-root-and-derived-medium-recovery.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`packages/storage/storage-json/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/storage/storage-json/src/index.ts) | package entry point | The source note names this file directly. Defines `JsonStorageBackend`, a construct named by the note. | `named-file, symbol-definition` |
| [`packages/storage/storage-json/src/format.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/storage/storage-json/src/format.ts) | runtime implementation | The source note names this file directly. | `named-file` |
| [`packages/storage/storage-domain/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/storage/storage-domain/src/index.ts) | package entry point | The source note names this file directly. | `named-file` |
| [`packages/session/session-persistence-jsonl/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/session/session-persistence-jsonl/src/index.ts) | package entry point | The source note names this file directly. | `named-file` |
| [`packages/util/home-paths/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/util/home-paths/src/index.ts) | package entry point | Core file in the package named by the note: `packages/util/home-paths`. Defines `dshHomePath`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/util/home-paths/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/util/home-paths/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/util/home-paths`. | `named-package-member` |
| [`packages/session/session-projection-cache/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/session/session-projection-cache/src/index.ts) | package entry point | Core file in the package named by the note: `packages/session/session-projection-cache`. | `named-package-member` |
| [`packages/session/session-projection-cache/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/session/session-projection-cache/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/session/session-projection-cache`. | `named-package-member` |
| [`apps/cli`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/apps/cli) | package or module directory | The note names this package or capability. | `named-package` |
| [`packages/util/home-paths`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/packages/util/home-paths) | package or module directory | The note names this package or capability. | `named-package` |
| [`packages/session/session-projection-cache`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/packages/session/session-projection-cache) | package or module directory | The note names this package or capability. | `named-package` |
| [`vendor/cordis/src/registry.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/cordis/src/registry.ts) | runtime implementation | Defines `resolve`, a construct named by the note. | `symbol-definition` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `json` | `const` | [`packages/client/modules/src/index.ts:169`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/modules/src/index.ts#L169) | `const json = JSON.stringify(graph).replaceAll('<', '\\u003c')` |
| `dsh` | `const` | [`packages/client/modules/src/index.ts:345`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/modules/src/index.ts#L345) | `const dsh = pkg.dsh` |
| `version` | `const` | [`packages/e2b/fs-e2b/src/index.ts:389`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/e2b/fs-e2b/src/index.ts#L389) | `const version = await this.writeAtomic(` |
| `recovery` | `const` | [`packages/fs/tool-fs-search/src/glob.ts:225`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/tool-fs-search/src/glob.ts#L225) | `const recovery = spillRef !== undefined` |
| `reject` | `function` | [`packages/goal/tool-goal/src/authority.ts:25`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/goal/tool-goal/src/authority.ts#L25) | `function reject(message: string, code = 'GOAL_TOOL_AUTHORITY_REQUIRED'): never {` |
| `workspace` | `const` | [`packages/host/apiproxy/src/api-proxy.ts:1519`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/src/api-proxy.ts#L1519) | `const workspace = workspaces.find(candidate => candidate.sessionIds.includes(ancestor.header.id))` |
| `reset` | `const` | [`packages/shell/tool-bash-persistent/src/index.ts:220`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/shell/tool-bash-persistent/src/index.ts#L220) | `const reset = async (owner: Agent, reason: string): Promise<void> => {` |
| `DomainError` | `class` | [`packages/storage/storage-domain/src/error.ts:34`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/storage/storage-domain/src/error.ts#L34) | `export class DomainError extends Error {` |
| `DomainSpec` | `interface` | [`packages/storage/storage-domain/src/spec.ts:35`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/storage/storage-domain/src/spec.ts#L35) | `export interface DomainSpec {` |
| `JsonStorageBackend` | `class` | [`packages/storage/storage-json/src/index.ts:38`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/storage/storage-json/src/index.ts#L38) | `export class JsonStorageBackend implements StorageBackend {` |
| `openJsonUnit` | `function` | [`packages/storage/storage-json/src/unit.ts:24`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/storage/storage-json/src/unit.ts#L24) | `export async function openJsonUnit(` |
| `KvFacet` | `interface` | [`packages/storage/storage/src/backend.ts:30`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/storage/storage/src/backend.ts#L30) | `export interface KvFacet {` |
| `StorageError` | `class` | [`packages/storage/storage/src/error.ts:20`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/storage/storage/src/error.ts#L20) | `export class StorageError extends Error {` |
| `dshHomePath` | `function` | [`packages/util/home-paths/src/index.ts:98`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/util/home-paths/src/index.ts#L98) | `export function dshHomePath(...segments: string[]): string {` |
| `resolve` | `function` | [`vendor/cordis/src/registry.ts:71`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/cordis/src/registry.ts#L71) | `export function resolve(inject: Inject \| null \| undefined, result: Dict = Object.create(null)) {` |

### Tests and executable evidence

- [`packages/typert/generator/tests/fixtures/type-model`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/typert/generator/tests/fixtures/type-model) — The note names this package or capability.
- [`packages/typert/generator/tests/fixtures/type-model/package.json`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/typert/generator/tests/fixtures/type-model/package.json) — Core file in the package named by the note: `packages/typert/generator/tests/fixtures/type-model`.
- [`apps/cli/tests/web-agent-presets.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/cli/tests/web-agent-presets.e2e.ts) — A test under the owning area exercises or imports `storageRoot`.
- [`packages/storage/storage/tests/contract.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/storage/storage/tests/contract.ts) — A test under the owning area exercises or imports `StorageError`.
- [`packages/util/home-paths/tests/home-paths.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/util/home-paths/tests/home-paths.spec.ts) — A test under the owning area exercises or imports `dshHomePath`. A test under the owning area exercises or imports `dsh-home-paths`.
- [`packages/storage/storage-domain/tests/domain.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/storage/storage-domain/tests/domain.spec.ts) — A test under the owning area exercises or imports `DomainError`.
- [`packages/storage/storage-json/tests/json-backend.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/storage/storage-json/tests/json-backend.spec.ts) — A test under the owning area exercises or imports `JsonStorageBackend`.
- [`packages/session/session-projection-cache/tests/cache.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/session/session-projection-cache/tests/cache.spec.ts) — A test under the owning area exercises or imports `stateVersion`. A test under the owning area exercises or imports `session_projcache`.
- Source verification intent: dsh launched from any directory reads and writes the same $DSH_HOME/storages/.json (default ~/.dsh/storages) --- already satisfied by the overlay expression; per-row overrides ride the personal config.yaml patch layer; the backend resolves a relative root once at construction (still to do). With a truncated, version-bumped, or schema-drifted session_projcache.json, the assembly boots clean: one warning names the discarded medium, the file is gone, the cache rebuilds through normal operation, and the cold listing column reappears as sessions are re-checkpointed.

## How to read the implementation

1. Start with [`packages/storage/storage-json/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/storage/storage-json/src/index.ts) because it has the strongest evidence link to the note.
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
- **`rust/process-discipline`** — Treat this as repository or release policy unless it changes a runtime contract. Do not add runtime machinery for a process-only lesson.
- **`rust/provider-adapter`** — Keep provider wire quirks inside adapters; the loop and durable event vocabulary remain provider-neutral.
- **`rust/quickjs-boundary`** — QuickJS holds orchestration variables; Rust/Tokio owns async execution and exposes only explicitly granted capabilities.
- **`rust/serde-validation`** — Parse untrusted input into strict serde types and validate semantic constraints before executing side effects.
- **`rust/tagged-enums`** — Use serde-tagged enums for a closed wire vocabulary. Add an explicit extension envelope only where runtime extension is truly required.
- **`rust/trait-seam`** — Use a small trait at the capability boundary; keep policy and model-facing schemas outside the backend implementation.
- **Status gate** — Proposal, not proof of shipped behavior. Reuse the pressure and acceptance criteria; verify whether any candidate code is partial scaffolding before porting it.

### Smallest safe implementation slice

1. Express the semantic contract as Rust types and one owning boundary.
2. Add the smallest adapter or service needed by the existing loop; do not add a new model-facing tool unless the four-tool surface cannot express the job.
3. Record durable evidence before deriving summaries or UI state.
4. Add one happy-path test, one interruption test, and one replay/restart test where state survives a turn.
5. Keep the patch reversible until the source note's strongest failure case is proven in the Rust harness.

## Search handles

- Tags: `class/architecture`, `concern/boundary`, `concern/compatibility`, `concern/discovery-routing`, `concern/evidence`, `concern/lifecycle`, `concern/ownership`, `concern/performance`, `concern/recovery`, `concern/schema-types`, `concern/simplification`, `domain/build-release`, `domain/configuration`, `domain/extensions`, `domain/filesystem`, `domain/llm`, `domain/session-state`, `domain/storage`, `domain/testing`, `lifecycle/proposed`, `mechanism/event-sourcing`, `mechanism/policy`, `mechanism/projection`, `mechanism/registry`
- Aliases: `json`, `dsh`, `version`, `recovery`, `reject`, `workspace`, `reset`, `DomainError`, `DomainSpec`, `JsonStorageBackend`, `openJsonUnit`, `KvFacet`, `StorageError`, `dshHomePath`
- Regex: `(?i)(json|version|recovery|reject|workspace|reset|DomainError|DomainSpec)`

```bash
rg -n --pcre2 "(?i)(json|version|recovery|reject|workspace|reset|DomainError|DomainSpec)" source-deepseek-harness
rg -l --fixed-strings "class/architecture" 03-agent-context
```

## Connected notes

- **`source-link`** — [0509. Domain KV storage capability seam and the workspace entity](0509-domain-kv-storage-capability-seam-and-the-workspace-entity.md): The source note links to this decision directly.
- **`source-link`** — [0511. Session projections and command lifecycle logging](0511-session-projections-and-command-lifecycle-logging.md): The source note links to this decision directly.
- **`shares-code-with`** — [0136. Workspace context instruction files](0136-workspace-context-instruction-files.md): Shares source implementation: `packages/util/home-paths/src/index.ts`, `packages/util/home-paths/src/invariant.ts`.
- **`shares-code-with`** — [0150. Expose agent session identity and JSONL location to tools and hooks](0150-expose-agent-session-identity-and-jsonl-location-to-tools-and-hooks.md): Shares source implementation: `packages/util/home-paths/src/index.ts`, `packages/util/home-paths/src/invariant.ts`.
- **`shares-code-with`** — [0233. SessionTelemetryBackend anonymous user id ($DSH_HOME/.anonymous-user-id) and the OTel Resource user.id](0233-sessiontelemetrybackend-anonymous-user-id-dsh-home-anonymous-user-id-and.md): Shares source implementation: `packages/util/home-paths`, `packages/util/home-paths/src/index.ts`.
- **`shares-code-with`** — [0513. Record last activity in the session index](0513-record-last-activity-in-the-session-index.md): Shares source implementation: `packages/session/session-projection-cache/src/index.ts`, `packages/session/session-projection-cache/src/invariant.ts`.
- **`shares-code-with`** — [0201. Cross-workspace session resume](0201-cross-workspace-session-resume.md): Shares source implementation: `apps/cli`, `packages/session/session-persistence-jsonl/src/index.ts`.
- **`shares-code-with`** — [0166. The dsh CLI and personal config overlays from the Harness home](0166-the-dsh-cli-and-personal-config-overlays-from-the-harness-home.md): Shares source implementation: `packages/util/home-paths/src/index.ts`, `packages/util/home-paths/src/invariant.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0512-storage-root-placement-and-derived-medium-recovery.md`.
