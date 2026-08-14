---
id: "dsh-note-0473"
title: "Omit runtime invariants from shipped dsh config"
status: "implemented"
class: "simplification"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/simplification/2026-08-03-omit-invariants-from-shipped-config.md"
implementation_evidence: "medium"
target_anchor: "append-only event and session store"
tags:
  - "class/simplification"
  - "concern/boundary"
  - "concern/evidence"
  - "concern/schema-types"
  - "concern/simplification"
  - "domain/build-release"
  - "domain/configuration"
  - "domain/observability"
  - "domain/session-state"
  - "domain/testing"
  - "domain/ui-interaction"
  - "lifecycle/implemented"
aliases:
  - "dsh"
  - "InvariantError"
  - "@deepseek-ai/dsh-invariants"
  - "./invariant"
  - "apps/cli/config/"
  - "@deepseek-ai/dsh-*/invariant"
  - "Omit runtime invariants from shipped dsh config"
  - "simplification"
  - "boundary"
  - "evidence"
  - "schema types"
  - "build release"
  - "configuration"
  - "observability"
search_regex: "(?i)(InvariantError|@deepseek\\-ai/dsh\\-invariants|\\./invariant|apps/cli/config/|@deepseek\\-ai/dsh\\-\\*/invariant|Omit[- ]runtime[- ]invariants[- ]from[- ]shipped[- ]dsh[- ]config|simplification|boundary)"
---

# 0473. Omit runtime invariants from shipped dsh config — implementation context

## Open this when

@deepseek-ai/dsh-invariants and package-owned ./invariant companions are optional development diagnostics. The shipped TUI mounted the service and four stateful companions while the shipped Web tree omitted them, so the two product surfaces had different diagnostic cost and failure behavior. A relational assertion failure could terminate an ordinary TUI run even though the always-on product boundary remained responsible for session validation and immutable history.

## Source decision

The shipped dsh configuration trees under apps/cli/config/ mount neither @deepseek-ai/dsh-invariants nor any package-owned ./invariant companion. The CLI package therefore carries no direct dependency on the invariant service. Invariant support remains available for focused tests, example bundles, generated SDK compositions, and custom deployments that opt into diagnostics explicitly. Session validation, snapshotting, freezing, and cited source-event validation remain always on and do not depend on the optional service, as defined by the source-owned immutability decision.

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/simplification/2026-08-03-omit-invariants-from-shipped-config.md](../02-notes/implemented/simplification/2026-08-03-omit-invariants-from-shipped-config.md)
- Pinned source: [.agents/notes/implemented/simplification/2026-08-03-omit-invariants-from-shipped-config.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/simplification/2026-08-03-omit-invariants-from-shipped-config.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`packages/runtime-diagnostics/invariants/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/runtime-diagnostics/invariants/src/index.ts) | package entry point | Core file in the package named by the note: `packages/runtime-diagnostics/invariants`. Defines `InvariantError`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/runtime-diagnostics/invariants/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/runtime-diagnostics/invariants/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/runtime-diagnostics/invariants`. | `named-package-member` |
| [`apps/cli/config`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/apps/cli/config) | package or module directory | The source note names this implementation area directly. | `named-directory` |
| [`apps/cli`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/apps/cli) | package or module directory | The note names this package or capability. | `named-package` |
| [`packages/runtime-diagnostics/invariants`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/packages/runtime-diagnostics/invariants) | package or module directory | The note names this package or capability. | `named-package` |
| [`packages/client/modules/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/modules/src/index.ts) | package entry point | Defines `dsh`, a construct named by the note. | `symbol-definition` |
| [`apps/cli/README.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/cli/README.md) | package contract and examples | Core file in the package named by the note: `apps/cli`. | `named-package-member` |
| [`apps/cli/package.json`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/cli/package.json) | composition and configuration | Core file in the package named by the note: `apps/cli`. | `named-package-member` |
| [`packages/runtime-diagnostics/invariants/README.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/runtime-diagnostics/invariants/README.md) | package contract and examples | Core file in the package named by the note: `packages/runtime-diagnostics/invariants`. | `named-package-member` |
| [`packages/runtime-diagnostics/invariants/package.json`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/runtime-diagnostics/invariants/package.json) | composition and configuration | Core file in the package named by the note: `packages/runtime-diagnostics/invariants`. | `named-package-member` |
| [`scripts/verify-cordis-config.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/verify-cordis-config.ts) | repository automation | Contains the exact code literal `apps/cli/config/` named by the note. | `exact-code-occurrence` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `dsh` | `const` | [`packages/client/modules/src/index.ts:345`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/modules/src/index.ts#L345) | `const dsh = pkg.dsh` |
| `InvariantError` | `class` | [`packages/runtime-diagnostics/invariants/src/index.ts:50`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/runtime-diagnostics/invariants/src/index.ts#L50) | `export class InvariantError extends Error {` |

### Tests and executable evidence

- [`packages/runtime-diagnostics/invariants/tests/service.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/runtime-diagnostics/invariants/tests/service.spec.ts) — A test under the owning area exercises or imports `InvariantError`.

## How to read the implementation

1. Start with [`packages/runtime-diagnostics/invariants/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/runtime-diagnostics/invariants/src/index.ts) because it has the strongest evidence link to the note.
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
- **`rust/newtype-ids`** — Represent identity domains with Rust newtypes; do not pass bare strings across session, call, turn, step, and agent boundaries.
- **`rust/process-discipline`** — Treat this as repository or release policy unless it changes a runtime contract. Do not add runtime machinery for a process-only lesson.
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

- Tags: `class/simplification`, `concern/boundary`, `concern/evidence`, `concern/schema-types`, `concern/simplification`, `domain/build-release`, `domain/configuration`, `domain/observability`, `domain/session-state`, `domain/testing`, `domain/ui-interaction`, `lifecycle/implemented`
- Aliases: `dsh`, `InvariantError`, `@deepseek-ai/dsh-invariants`, `./invariant`, `apps/cli/config/`, `@deepseek-ai/dsh-*/invariant`, `Omit runtime invariants from shipped dsh config`, `simplification`, `boundary`, `evidence`, `schema types`, `build release`, `configuration`, `observability`
- Regex: `(?i)(InvariantError|@deepseek\-ai/dsh\-invariants|\./invariant|apps/cli/config/|@deepseek\-ai/dsh\-\*/invariant|Omit[- ]runtime[- ]invariants[- ]from[- ]shipped[- ]dsh[- ]config|simplification|boundary)`

```bash
rg -n --pcre2 "(?i)(InvariantError|@deepseek\\-ai/dsh\\-invariants|\\./invariant|apps/cli/config/|@deepseek\\-ai/dsh\\-\\*/invariant|Omit[- ]runtime[- ]invariants[- ]from[- ]shipped[- ]dsh[- ]config|simplification|boundary)" source-deepseek-harness
rg -l --fixed-strings "class/simplification" 03-agent-context
```

## Connected notes

- **`source-link`** — [0002. Source-owned session immutability and dev-mode invariants](0002-source-owned-session-immutability-and-dev-mode-invariants.md): The source note links to this decision directly.
- **`shares-code-with`** — [0676. Explicit-config dsh entrypoint](0676-explicit-config-dsh-entrypoint.md): Shares source implementation: `apps/cli`, `apps/cli/README.md`.
- **`shares-code-with`** — [0527. Artifact-first NPM baseline publication](0527-artifact-first-npm-baseline-publication.md): Shares source implementation: `apps/cli`, `apps/cli/README.md`.
- **`shares-code-with`** — [0579. Product-level TUI session resume](0579-product-level-tui-session-resume.md): Shares source implementation: `apps/cli`, `apps/cli/README.md`.
- **`shares-code-with`** — [0402. Product-first root README](0402-product-first-root-readme.md): Shares source implementation: `apps/cli`, `apps/cli/README.md`.
- **`shares-code-with`** — [0604. Versioned TUI first-run welcome](0604-versioned-tui-first-run-welcome.md): Shares source implementation: `apps/cli`, `apps/cli/README.md`.
- **`shares-code-with`** — [0485. Source run without a managed installer](0485-source-run-without-a-managed-installer.md): Shares source implementation: `apps/cli`, `apps/cli/README.md`.
- **`shares-code-with`** — [0647. the installer adopts an existing checkout into the managed layout](0647-the-installer-adopts-an-existing-checkout-into-the-managed-layout.md): Shares source implementation: `apps/cli`, `apps/cli/README.md`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0473-omit-runtime-invariants-from-shipped-dsh-config.md`.
