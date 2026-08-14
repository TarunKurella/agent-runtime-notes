---
id: "dsh-note-0047"
title: "Package-owned invariant service contract"
status: "implemented"
class: "architecture"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/architecture/2026-07-19-package-owned-invariant-service.md"
implementation_evidence: "medium"
target_anchor: "repository tests and release policy"
tags:
  - "class/architecture"
  - "concern/boundary"
  - "concern/discovery-routing"
  - "concern/evidence"
  - "concern/lifecycle"
  - "concern/ownership"
  - "concern/schema-types"
  - "domain/build-release"
  - "domain/configuration"
  - "domain/extensions"
  - "domain/filesystem"
  - "domain/llm"
  - "domain/observability"
  - "domain/session-state"
  - "domain/storage"
  - "domain/testing"
  - "domain/ui-interaction"
  - "lifecycle/implemented"
  - "mechanism/generation"
  - "mechanism/policy"
  - "mechanism/projection"
  - "mechanism/registry"
aliases:
  - "fail"
  - "dsh"
  - "inject"
  - "apply"
  - "scopeTarget"
  - "enabled"
  - "InvariantError"
  - "packageName"
  - "@deepseek-ai/dsh-invariants"
  - "ctx.invariants"
  - "./invariant"
  - "package_allowlist: []"
  - "package_blocklist: []"
  - "new RegExp"
search_regex: "(?i)(fail|inject|apply|scopeTarget|enabled|InvariantError|packageName|@deepseek\\-ai/dsh\\-invariants)"
---

# 0047. Package-owned invariant service contract — implementation context

## Open this when

Runtime invariant checks span session traces, agent state, scoped dispatch, and request reconstruction. Putting all checks in one diagnostics package makes that package import product vocabularies from unrelated domains, centralizes tests away from their owners, and requires the central package to change whenever a product package adds or removes a check. Deployments that opt into diagnostics need more than presence or absence of one plugin. Such a composition carries the known invariant contributions while permitting a global off switch and package-selective diagnostics.

## Source decision

@deepseek-ai/dsh-invariants is a product-independent Cordis service plugin that registers ctx.invariants. It owns configuration, registration uniqueness, child-fiber lifecycle, and package-attributed failures. It imports no session, agent, scope, or agent-loop package and contains none of their checks. Every workspace package publishes a ./invariant companion plugin that registers its exact full npm name. A companion checks a meaningful event or mutable-data relationship when its owner has one; otherwise it carries an owner-specific explanation for its empty installer.

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/architecture/2026-07-19-package-owned-invariant-service.md](../02-notes/implemented/architecture/2026-07-19-package-owned-invariant-service.md)
- Pinned source: [.agents/notes/implemented/architecture/2026-07-19-package-owned-invariant-service.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/architecture/2026-07-19-package-owned-invariant-service.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`packages/core/agent/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent/src/index.ts) | package entry point | Core file in the package named by the note: `packages/core/agent`. | `named-package-member` |
| [`packages/core/agent/src/types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent/src/types.ts) | public types and contract | Core file in the package named by the note: `packages/core/agent`. | `named-package-member` |
| [`packages/core/scope/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/scope/src/index.ts) | package entry point | Core file in the package named by the note: `packages/core/scope`. Defines `scopeTarget`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/core/session/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/index.ts) | package entry point | Core file in the package named by the note: `packages/core/session`. | `named-package-member` |
| [`packages/core/session/src/types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/types.ts) | public types and contract | Core file in the package named by the note: `packages/core/session`. | `named-package-member` |
| [`packages/core/agent/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/core/agent`. Defines `inject`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/core/scope/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/scope/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/core/scope`. Defines `inject`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/core/agent-loop/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent-loop/src/index.ts) | package entry point | Core file in the package named by the note: `packages/core/agent-loop`. | `named-package-member` |
| [`packages/core/session/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/core/session`. Defines `inject`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/core/agent-loop/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent-loop/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/core/agent-loop`. Defines `inject`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/runtime-diagnostics/invariants/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/runtime-diagnostics/invariants/src/index.ts) | package entry point | Core file in the package named by the note: `packages/runtime-diagnostics/invariants`. Defines `InvariantError`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/runtime-diagnostics/invariants/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/runtime-diagnostics/invariants/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/runtime-diagnostics/invariants`. Defines `inject`, a construct named by the note. | `named-package-member, symbol-definition` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `fail` | `function` | [`packages/bundle/headless/src/index.ts:85`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/bundle/headless/src/index.ts#L85) | `function fail(io: HeadlessIo, error: unknown): void {` |
| `dsh` | `const` | [`packages/client/modules/src/index.ts:345`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/modules/src/index.ts#L345) | `const dsh = pkg.dsh` |
| `inject` | `const` | [`packages/core/agent-loop/src/invariant.ts:16`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent-loop/src/invariant.ts#L16) | `export const inject = ['invariants']` |
| `apply` | `const` | [`packages/core/agent-loop/src/invariant.ts:62`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent-loop/src/invariant.ts#L62) | `export const apply = (ctx: Context): Promise<() => void> =>` |
| `inject` | `const` | [`packages/core/agent/src/invariant.ts:12`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent/src/invariant.ts#L12) | `export const inject = ['invariants']` |
| `apply` | `const` | [`packages/core/agent/src/invariant.ts:31`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent/src/invariant.ts#L31) | `export const apply = (ctx: Context): Promise<() => void> =>` |
| `scopeTarget` | `function` | [`packages/core/scope/src/index.ts:170`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/scope/src/index.ts#L170) | `export function scopeTarget<T extends object>(base: T, key: ScopeKey \| undefined): Scoped<T> {` |
| `inject` | `const` | [`packages/core/scope/src/invariant.ts:13`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/scope/src/invariant.ts#L13) | `export const inject = ['invariants']` |
| `apply` | `const` | [`packages/core/scope/src/invariant.ts:40`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/scope/src/invariant.ts#L40) | `export const apply = (ctx: Context): Promise<() => void> =>` |
| `inject` | `const` | [`packages/core/session/src/invariant.ts:20`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/invariant.ts#L20) | `export const inject = ['invariants']` |
| `apply` | `const` | [`packages/core/session/src/invariant.ts:249`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/invariant.ts#L249) | `export const apply = (ctx: Context): Promise<() => void> =>` |
| `enabled` | `const` | [`packages/mcp/mcp-client/src/connection.ts:71`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/mcp/mcp-client/src/connection.ts#L71) | `const enabled = config?.enabled ?? RECONNECT_DEFAULTS.enabled` |
| `InvariantError` | `class` | [`packages/runtime-diagnostics/invariants/src/index.ts:50`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/runtime-diagnostics/invariants/src/index.ts#L50) | `export class InvariantError extends Error {` |
| `inject` | `const` | [`packages/runtime-diagnostics/invariants/src/invariant.ts:15`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/runtime-diagnostics/invariants/src/invariant.ts#L15) | `export const inject = ['invariants']` |
| `apply` | `const` | [`packages/runtime-diagnostics/invariants/src/invariant.ts:28`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/runtime-diagnostics/invariants/src/invariant.ts#L28) | `export const apply = (ctx: Context): Promise<() => void> =>` |
| `packageName` | `const` | [`packages/typert/registry/src/service.ts:544`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/typert/registry/src/service.ts#L544) | `const packageName = key.slice(0, hash)` |

### Tests and executable evidence

- [`apps/cli/tests/built-bin.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/cli/tests/built-bin.e2e.ts) — A test under the owning area exercises or imports `inject`.
- [`apps/cli/tests/windows-shell.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/cli/tests/windows-shell.spec.ts) — A test under the owning area exercises or imports `enabled`.
- [`apps/cli/tests/dsh-badge.snapshot.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/cli/tests/dsh-badge.snapshot.ts) — A test under the owning area exercises or imports `enabled`.
- [`packages/core/scope/tests/scope.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/scope/tests/scope.spec.ts) — A test under the owning area exercises or imports `scopeTarget`.
- [`packages/core/scope/tests/invariant.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/scope/tests/invariant.spec.ts) — A test under the owning area exercises or imports `scopeTarget`.
- [`packages/core/agent/tests/invariant.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent/tests/invariant.spec.ts) — A test under the owning area exercises or imports `scopeTarget`.
- [`apps/cli/tests/fixtures/dsh-badge/snapshot.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/cli/tests/fixtures/dsh-badge/snapshot.ts) — A test under the owning area exercises or imports `inject`.
- [`packages/core/session/tests/invariant.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/tests/invariant.spec.ts) — A test under the owning area exercises or imports `InvariantError`. A test under the owning area exercises or imports `scopeTarget`.
- Source verification intent: Service tests cover defaults, global disablement, allow/block selection, blocklist precedence, anchoring, unanchored matching, case sensitivity, invalid configuration, zero-match patterns, late registration, duplicate ownership, disposal, rollback, and HMR re-registration. Owners with executable checks keep positive and negative behavior beside the companion source. Composition tests cover standard-spine forwarding and generated SDK entries. Loader tests preserve each companion namespace, while built plain-Node smokes exercise the compiled subpath exports.

## How to read the implementation

1. Start with [`packages/core/agent/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent/src/index.ts) because it has the strongest evidence link to the note.
2. Read public types and entry points before implementation details. Write down the contract in plain language.
3. Follow the named constructs into their callers and tests. Confirm which component can actually enforce the rule.
4. Reconstruct one failure path: cancellation, crash, retry, stale state, partial write, or incompatible provider input—whichever the note is protecting against.
5. Only then map the contract into the current Rust repository. Target paths must come from that repository, never from guesswork in this wiki.

## Rust harness port

**Likely integration point:** repository tests and release policy.

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

### Smallest safe implementation slice

1. Express the semantic contract as Rust types and one owning boundary.
2. Add the smallest adapter or service needed by the existing loop; do not add a new model-facing tool unless the four-tool surface cannot express the job.
3. Record durable evidence before deriving summaries or UI state.
4. Add one happy-path test, one interruption test, and one replay/restart test where state survives a turn.
5. Keep the patch reversible until the source note's strongest failure case is proven in the Rust harness.

## Search handles

- Tags: `class/architecture`, `concern/boundary`, `concern/discovery-routing`, `concern/evidence`, `concern/lifecycle`, `concern/ownership`, `concern/schema-types`, `domain/build-release`, `domain/configuration`, `domain/extensions`, `domain/filesystem`, `domain/llm`, `domain/observability`, `domain/session-state`, `domain/storage`, `domain/testing`, `domain/ui-interaction`, `lifecycle/implemented`, `mechanism/generation`, `mechanism/policy`, `mechanism/projection`, `mechanism/registry`
- Aliases: `fail`, `dsh`, `inject`, `apply`, `scopeTarget`, `enabled`, `InvariantError`, `packageName`, `@deepseek-ai/dsh-invariants`, `ctx.invariants`, `./invariant`, `package_allowlist: []`, `package_blocklist: []`, `new RegExp`
- Regex: `(?i)(fail|inject|apply|scopeTarget|enabled|InvariantError|packageName|@deepseek\-ai/dsh\-invariants)`

```bash
rg -n --pcre2 "(?i)(fail|inject|apply|scopeTarget|enabled|InvariantError|packageName|@deepseek\\-ai/dsh\\-invariants)" source-deepseek-harness
rg -l --fixed-strings "class/architecture" 03-agent-context
```

## Connected notes

- **`source-link`** — [0046. Meaningful package invariant contracts](0046-meaningful-package-invariant-contracts.md): The source note links to this decision directly.
- **`source-link`** — [0473. Omit runtime invariants from shipped dsh config](0473-omit-runtime-invariants-from-shipped-dsh-config.md): The source note links to this decision directly.
- **`shares-code-with`** — [0002. Source-owned session immutability and dev-mode invariants](0002-source-owned-session-immutability-and-dev-mode-invariants.md): Shares source implementation: `packages/core/agent-loop/src/index.ts`, `packages/core/agent/src/index.ts`.
- **`shares-code-with`** — [0035. Agent-scope runtime design and correctness](0035-agent-scope-runtime-design-and-correctness.md): Shares source implementation: `packages/core/agent/src/index.ts`, `packages/core/agent/src/invariant.ts`.
- **`shares-code-with`** — [0038. Initiating Agent scope over AsyncLocalStorage](0038-initiating-agent-scope-over-asynclocalstorage.md): Shares source implementation: `packages/core/agent/src/index.ts`, `packages/core/agent/src/invariant.ts`.
- **`shares-code-with`** — [0150. Expose agent session identity and JSONL location to tools and hooks](0150-expose-agent-session-identity-and-jsonl-location-to-tools-and-hooks.md): Shares source implementation: `packages/core/agent/src/index.ts`, `packages/core/agent/src/invariant.ts`.
- **`shares-code-with`** — [0265. Settlement delivery belongs to the continuation manager](0265-settlement-delivery-belongs-to-the-continuation-manager.md): Shares source implementation: `packages/core/agent/src/index.ts`, `packages/core/agent/src/invariant.ts`.
- **`shares-code-with`** — [0112. Per-preset standing mounts over a scope parent chain](0112-per-preset-standing-mounts-over-a-scope-parent-chain.md): Shares source implementation: `packages/core/agent/src/index.ts`, `packages/core/agent/src/invariant.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0047-package-owned-invariant-service-contract.md`.
