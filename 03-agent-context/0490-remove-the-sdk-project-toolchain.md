---
id: "dsh-note-0490"
title: "Remove the SDK project toolchain"
status: "implemented"
class: "simplification"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/simplification/2026-08-11-remove-sdk-project-toolchain.md"
implementation_evidence: "medium"
target_anchor: "read/write/patch filesystem capability"
tags:
  - "class/simplification"
  - "concern/boundary"
  - "concern/discovery-routing"
  - "concern/evidence"
  - "concern/lifecycle"
  - "concern/simplification"
  - "domain/build-release"
  - "domain/extensions"
  - "domain/filesystem"
  - "domain/llm"
  - "domain/multi-agent"
  - "domain/observability"
  - "domain/protocols"
  - "domain/testing"
  - "lifecycle/implemented"
  - "mechanism/generation"
aliases:
  - "@deepseek-ai/create-sdk"
  - "@deepseek-ai/dsh-scripts"
  - "dsh-sdk"
  - "@deepseek-ai/dsh-helper"
  - "@deepseek-ai/dsh-telemetry"
  - "scaffold/"
  - "@deepseek-ai/dsh-sdk-client"
  - "@deepseek-ai/dsh-sdk-protocol"
  - "@deepseek-ai/dsh-sdk-jsonrpc-server"
  - "packages/scaffold/"
  - "packages/sdk/"
  - "cordis.yml"
  - "SDK"
  - "Remove the SDK project toolchain"
search_regex: "(?i)(@deepseek\\-ai/create\\-sdk|@deepseek\\-ai/dsh\\-scripts|dsh\\-sdk|@deepseek\\-ai/dsh\\-helper|@deepseek\\-ai/dsh\\-telemetry|scaffold/|@deepseek\\-ai/dsh\\-sdk\\-client|@deepseek\\-ai/dsh\\-sdk\\-protocol)"
---

# 0490. Remove the SDK project toolchain — implementation context

## Open this when

The repository carried an unreleased developer-project product with no consumers. @deepseek-ai/create-sdk generated an editable Cordis project, @deepseek-ai/dsh-scripts supplied its dsh-sdk development, build, start, configuration, and plugin-install commands, @deepseek-ai/dsh-helper coordinated feature definitions and multi-file project edits, and @deepseek-ai/dsh-telemetry reported launcher activity. The design aimed to keep generated projects editable while giving creation and later configuration one definition of dependencies, Cordis entries, environment placeholders, and owned files.

## Source decision

The SDK project toolchain is deleted. The @deepseek-ai/create-sdk, @deepseek-ai/dsh-scripts, @deepseek-ai/dsh-helper, and @deepseek-ai/dsh-telemetry packages, their binaries, tests, templates, feature catalog, project-editing model, package-manager support, launcher telemetry, and repository creation skill have no replacement or compatibility layer. Their workspace, build, test, packaging, documentation-generator, vendoring-rescope, and dependency records are removed with them. The runtime SDK remains.

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/simplification/2026-08-11-remove-sdk-project-toolchain.md](../02-notes/implemented/simplification/2026-08-11-remove-sdk-project-toolchain.md)
- Pinned source: [.agents/notes/implemented/simplification/2026-08-11-remove-sdk-project-toolchain.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/simplification/2026-08-11-remove-sdk-project-toolchain.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`packages/sdk/client/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/sdk/client/src/index.ts) | package entry point | Core file in the package named by the note: `packages/sdk/client`. | `named-package-member` |
| [`packages/sdk/client/src/types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/sdk/client/src/types.ts) | public types and contract | Core file in the package named by the note: `packages/sdk/client`. | `named-package-member` |
| [`packages/sdk/server/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/sdk/server/src/index.ts) | package entry point | Core file in the package named by the note: `packages/sdk/server`. | `named-package-member` |
| [`packages/sdk/protocol/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/sdk/protocol/src/index.ts) | package entry point | Core file in the package named by the note: `packages/sdk/protocol`. | `named-package-member` |
| [`packages/sdk/protocol/src/types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/sdk/protocol/src/types.ts) | public types and contract | Core file in the package named by the note: `packages/sdk/protocol`. | `named-package-member` |
| [`packages/sdk/client/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/sdk/client/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/sdk/client`. | `named-package-member` |
| [`packages/sdk/server/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/sdk/server/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/sdk/server`. | `named-package-member` |
| [`packages/sdk/protocol/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/sdk/protocol/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/sdk/protocol`. | `named-package-member` |
| [`packages/sdk/README.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/sdk/README.md) | package contract and examples | Entry point or contract under the directory named by the note: `packages/sdk`. | `named-directory-member` |
| [`packages/sdk`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/packages/sdk) | package or module directory | The source note names this implementation area directly. | `named-directory` |
| [`packages/sdk/client`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/packages/sdk/client) | package or module directory | The note names this package or capability. | `named-package` |
| [`packages/sdk/server`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/packages/sdk/server) | package or module directory | The note names this package or capability. | `named-package` |

### Code constructs named by the note

The note names behavior or policy but no uniquely resolvable code declaration. Search by its aliases and inspect the listed package entry points.

### Tests and executable evidence

- No test file was tied to this note with enough confidence. Read the note's verification section and search the owning package's `tests/` directory.
- Source verification intent: The workspace contains none of the four deleted package names or either removed command product. Package aggregates, source path maps, package metadata, test collection, publication constraints, generated catalogs, dependency notices, and the lockfile resolve only the three runtime SDK packages under packages/sdk/. The runtime SDK package tests, its built server smoke, TypeScript consumers, repository documentation gates, build, and hygiene checks pin the surviving behavior and the absence of stale package paths.

## How to read the implementation

1. Start with [`packages/sdk/client/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/sdk/client/src/index.ts) because it has the strongest evidence link to the note.
2. Read public types and entry points before implementation details. Write down the contract in plain language.
3. Follow the named constructs into their callers and tests. Confirm which component can actually enforce the rule.
4. Reconstruct one failure path: cancellation, crash, retry, stale state, partial write, or incompatible provider input—whichever the note is protecting against.
5. Only then map the contract into the current Rust repository. Target paths must come from that repository, never from guesswork in this wiki.

## Rust harness port

**Likely integration point:** read/write/patch filesystem capability.

Preserve the behavior and ownership rule. Do not mechanically translate Cordis, TypeScript declaration merging, Node streams, or package topology into Rust.

Relevant standing rules:

- **`rust/capability-security`** — Make granted capabilities explicit. On the Windows no-admin path, probe support at runtime and fail closed rather than silently running unsandboxed.
- **`rust/four-tool-surface`** — Preserve the four model tools: read, write, patch, and exec. Reuse internal capabilities without automatically exposing new model-facing tools.
- **`rust/process-discipline`** — Treat this as repository or release policy unless it changes a runtime contract. Do not add runtime machinery for a process-only lesson.
- **`rust/provider-adapter`** — Keep provider wire quirks inside adapters; the loop and durable event vocabulary remain provider-neutral.
- **`rust/quickjs-boundary`** — QuickJS holds orchestration variables; Rust/Tokio owns async execution and exposes only explicitly granted capabilities.
- **`rust/trait-seam`** — Use a small trait at the capability boundary; keep policy and model-facing schemas outside the backend implementation.

### Smallest safe implementation slice

1. Express the semantic contract as Rust types and one owning boundary.
2. Add the smallest adapter or service needed by the existing loop; do not add a new model-facing tool unless the four-tool surface cannot express the job.
3. Record durable evidence before deriving summaries or UI state.
4. Add one happy-path test, one interruption test, and one replay/restart test where state survives a turn.
5. Keep the patch reversible until the source note's strongest failure case is proven in the Rust harness.

## Search handles

- Tags: `class/simplification`, `concern/boundary`, `concern/discovery-routing`, `concern/evidence`, `concern/lifecycle`, `concern/simplification`, `domain/build-release`, `domain/extensions`, `domain/filesystem`, `domain/llm`, `domain/multi-agent`, `domain/observability`, `domain/protocols`, `domain/testing`, `lifecycle/implemented`, `mechanism/generation`
- Aliases: `@deepseek-ai/create-sdk`, `@deepseek-ai/dsh-scripts`, `dsh-sdk`, `@deepseek-ai/dsh-helper`, `@deepseek-ai/dsh-telemetry`, `scaffold/`, `@deepseek-ai/dsh-sdk-client`, `@deepseek-ai/dsh-sdk-protocol`, `@deepseek-ai/dsh-sdk-jsonrpc-server`, `packages/scaffold/`, `packages/sdk/`, `cordis.yml`, `SDK`, `Remove the SDK project toolchain`
- Regex: `(?i)(@deepseek\-ai/create\-sdk|@deepseek\-ai/dsh\-scripts|dsh\-sdk|@deepseek\-ai/dsh\-helper|@deepseek\-ai/dsh\-telemetry|scaffold/|@deepseek\-ai/dsh\-sdk\-client|@deepseek\-ai/dsh\-sdk\-protocol)`

```bash
rg -n --pcre2 "(?i)(@deepseek\\-ai/create\\-sdk|@deepseek\\-ai/dsh\\-scripts|dsh\\-sdk|@deepseek\\-ai/dsh\\-helper|@deepseek\\-ai/dsh\\-telemetry|scaffold/|@deepseek\\-ai/dsh\\-sdk\\-client|@deepseek\\-ai/dsh\\-sdk\\-protocol)" source-deepseek-harness
rg -l --fixed-strings "class/simplification" 03-agent-context
```

## Connected notes

- **`source-link`** — [0126. Repository naming contract and pre-release rename ledger](0126-repository-naming-contract-and-pre-release-rename-ledger.md): The source note links to this decision directly.
- **`shares-code-with`** — [0075. Regroup packages/ by measured clustering](0075-regroup-packages-by-measured-clustering.md): Shares source implementation: `packages/sdk/protocol/src/index.ts`, `packages/sdk/protocol/src/types.ts`.
- **`shares-code-with`** — [0195. TypeScript SDK client and the SDK subagent backend](0195-typescript-sdk-client-and-the-sdk-subagent-backend.md): Shares source implementation: `packages/sdk/client/src/index.ts`, `packages/sdk/client/src/types.ts`.
- **`shares-code-with`** — [0529. Make JSON-RPC completion and transport directional](0529-make-json-rpc-completion-and-transport-directional.md): Shares source implementation: `packages/sdk/protocol/src/index.ts`, `packages/sdk/protocol/src/invariant.ts`.
- **`shares-code-with`** — [0542. Dependency swaps rejected by the 2026-07 NIH audit](0542-dependency-swaps-rejected-by-the-2026-07-nih-audit.md): Shares source implementation: `packages/sdk/server/src/index.ts`, `packages/sdk/server/src/invariant.ts`.
- **`shares-code-with`** — [0293. Minimal profiles use the bare two-tool runtime](0293-minimal-profiles-use-the-bare-two-tool-runtime.md): Shares source implementation: `packages/sdk/server/src/index.ts`, `packages/sdk/server/src/invariant.ts`.
- **`shares-code-with`** — [0456. Remove the stdio and Echo agents](0456-remove-the-stdio-and-echo-agents.md): Shares source implementation: `packages/sdk/server/src/index.ts`, `packages/sdk/server/src/invariant.ts`.
- **`shares-code-with`** — [0306. Classify pi-ai transport truncations from flattened message text](0306-classify-pi-ai-transport-truncations-from-flattened-message-text.md): Shares source implementation: `packages/sdk/server/src/index.ts`, `packages/sdk/server/src/invariant.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0490-remove-the-sdk-project-toolchain.md`.
