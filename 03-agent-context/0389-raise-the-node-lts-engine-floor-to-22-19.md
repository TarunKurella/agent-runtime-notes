---
id: "dsh-note-0389"
title: "Raise the Node LTS engine floor to 22.19"
status: "implemented"
class: "process"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/process/2026-07-06-node-engine-floor.md"
implementation_evidence: "high"
target_anchor: "repository tests and engineering policy"
tags:
  - "class/process"
  - "concern/boundary"
  - "concern/compatibility"
  - "concern/evidence"
  - "concern/lifecycle"
  - "concern/ownership"
  - "domain/build-release"
  - "domain/filesystem"
  - "domain/llm"
  - "domain/session-state"
  - "domain/storage"
  - "domain/testing"
  - "lifecycle/implemented"
  - "mechanism/adapter"
  - "mechanism/policy"
aliases:
  - "node"
  - "engines.node"
  - "pnpm install --engine-strict"
  - "^22.19.0 || >=24.0.0"
  - "['22.19', 24, 26]"
  - "packages/session/session-persistence-sqlite"
  - "import { DatabaseSync } from 'node:sqlite"
  - "--experimental-sqlite"
  - "examples/headless-agent/tests/keyless-smoke.e2e.ts"
  - ".ts"
  - "cli-mock-llm.ts"
  - "--experimental-strip-types"
  - "@deepseek-ai/dsh-llm-pi-ai"
  - "@earendil-works/pi-ai@0.79.3"
search_regex: "(?i)(node|engines\\.node|pnpm[- ]install[- ]\\-\\-engine\\-strict|\\^22\\.19\\.0[- ]\\|\\|[- ]>=24\\.0\\.0|\\['22\\.19',[- ]24,[- ]26\\]|packages/session/session\\-persistence\\-sqlite|import[- ]\\{[- ]DatabaseSync[- ]\\}[- ]from[- ]'node:sqlite|\\-\\-experimental\\-sqlite)"
---

# 0389. Raise the Node LTS engine floor to 22.19 — implementation context

## Open this when

The Node 22 branch of the root engines.node range is a contract for the installed workspace, not only for the runtime APIs the harness source calls directly. It must be no lower than package engines.node declarations for dependencies the workspace installs on that branch; otherwise pnpm install --engine-strict fails at an advertised LTS version, and non-strict installs run outside a dependency's supported runtime.

## Source decision

Two Node features gate the source runtime: node:sqlite --- packages/session/session-persistence-sqlite does a top-level import { DatabaseSync } from 'node:sqlite'. The module dropped its --experimental-sqlite flag requirement at 22.13 (LTS) and 23.4 (Current); before those, importing it throws at load. Native TypeScript type-stripping --- the built-mode examples/headless-agent/tests/keyless-smoke.e2e.ts smoke boots its unexported .ts driver under plain node (no tsx) and loads the example's .ts test adapter (cli-mock-llm.ts).

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/process/2026-07-06-node-engine-floor.md](../02-notes/implemented/process/2026-07-06-node-engine-floor.md)
- Pinned source: [.agents/notes/implemented/process/2026-07-06-node-engine-floor.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/process/2026-07-06-node-engine-floor.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`packages/llm/llm-pi-ai/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm-pi-ai/src/index.ts) | package entry point | Core file in the package named by the note: `packages/llm/llm-pi-ai`. | `named-package-member` |
| [`packages/llm/llm-pi-ai/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm-pi-ai/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/llm/llm-pi-ai`. | `named-package-member` |
| [`packages/session/session-persistence-sqlite/README.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/session/session-persistence-sqlite/README.md) | package contract and examples | Entry point or contract under the directory named by the note: `packages/session/session-persistence-sqlite`. | `named-directory-member` |
| [`packages/session/session-persistence-sqlite/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/session/session-persistence-sqlite/src/index.ts) | package entry point | Entry point or contract under the directory named by the note: `packages/session/session-persistence-sqlite`. | `named-directory-member` |
| [`packages/session/session-persistence-sqlite/package.json`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/session/session-persistence-sqlite/package.json) | composition and configuration | Entry point or contract under the directory named by the note: `packages/session/session-persistence-sqlite`. | `named-directory-member` |
| [`packages/session/session-persistence-sqlite/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/session/session-persistence-sqlite/src/invariant.ts) | runtime contract checks | Entry point or contract under the directory named by the note: `packages/session/session-persistence-sqlite`. | `named-directory-member` |
| [`packages/session/session-persistence-sqlite`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/packages/session/session-persistence-sqlite) | package or module directory | The source note names this implementation area directly. | `named-directory` |
| [`packages/llm/llm-pi-ai`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm-pi-ai) | package or module directory | The note names this package or capability. | `named-package` |
| [`packages/core/tools/src/schema.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/schema.ts) | runtime implementation | Defines `node`, a construct named by the note. | `symbol-definition` |
| [`packages/llm/llm-pi-ai/README.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm-pi-ai/README.md) | package contract and examples | Core file in the package named by the note: `packages/llm/llm-pi-ai`. | `named-package-member` |
| [`packages/llm/llm-pi-ai/package.json`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm-pi-ai/package.json) | composition and configuration | Core file in the package named by the note: `packages/llm/llm-pi-ai`. | `named-package-member` |
| [`pnpm-lock.yaml`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/pnpm-lock.yaml) | composition and configuration | Contains the exact code literal `packages/session/session-persistence-sqlite` named by the note. | `exact-code-occurrence` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `node` | `const` | [`packages/core/tools/src/schema.ts:336`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/schema.ts#L336) | `const node: JsonSchemaNode = {}` |

### Tests and executable evidence

- [`examples/headless-agent/tests/keyless-smoke.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/examples/headless-agent/tests/keyless-smoke.e2e.ts) — The source note names this file directly.

## How to read the implementation

1. Start with [`packages/llm/llm-pi-ai/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm-pi-ai/src/index.ts) because it has the strongest evidence link to the note.
2. Read public types and entry points before implementation details. Write down the contract in plain language.
3. Follow the named constructs into their callers and tests. Confirm which component can actually enforce the rule.
4. Reconstruct one failure path: cancellation, crash, retry, stale state, partial write, or incompatible provider input—whichever the note is protecting against.
5. Only then map the contract into the current Rust repository. Target paths must come from that repository, never from guesswork in this wiki.

## Rust harness port

**Likely integration point:** repository tests and engineering policy.

Preserve the behavior and ownership rule. Do not mechanically translate Cordis, TypeScript declaration merging, Node streams, or package topology into Rust.

Relevant standing rules:

- **`rust/append-log`** — Keep durable facts append-only and derive views from them. Separate the synchronous fact boundary from asynchronous persistence.
- **`rust/capability-security`** — Make granted capabilities explicit. On the Windows no-admin path, probe support at runtime and fail closed rather than silently running unsandboxed.
- **`rust/four-tool-surface`** — Preserve the four model tools: read, write, patch, and exec. Reuse internal capabilities without automatically exposing new model-facing tools.
- **`rust/process-discipline`** — Treat this as repository or release policy unless it changes a runtime contract. Do not add runtime machinery for a process-only lesson.
- **`rust/provider-adapter`** — Keep provider wire quirks inside adapters; the loop and durable event vocabulary remain provider-neutral.
- **`rust/trait-seam`** — Use a small trait at the capability boundary; keep policy and model-facing schemas outside the backend implementation.

### Smallest safe implementation slice

1. Express the semantic contract as Rust types and one owning boundary.
2. Add the smallest adapter or service needed by the existing loop; do not add a new model-facing tool unless the four-tool surface cannot express the job.
3. Record durable evidence before deriving summaries or UI state.
4. Add one happy-path test, one interruption test, and one replay/restart test where state survives a turn.
5. Keep the patch reversible until the source note's strongest failure case is proven in the Rust harness.

## Search handles

- Tags: `class/process`, `concern/boundary`, `concern/compatibility`, `concern/evidence`, `concern/lifecycle`, `concern/ownership`, `domain/build-release`, `domain/filesystem`, `domain/llm`, `domain/session-state`, `domain/storage`, `domain/testing`, `lifecycle/implemented`, `mechanism/adapter`, `mechanism/policy`
- Aliases: `node`, `engines.node`, `pnpm install --engine-strict`, `^22.19.0 || >=24.0.0`, `['22.19', 24, 26]`, `packages/session/session-persistence-sqlite`, `import { DatabaseSync } from 'node:sqlite`, `--experimental-sqlite`, `examples/headless-agent/tests/keyless-smoke.e2e.ts`, `.ts`, `cli-mock-llm.ts`, `--experimental-strip-types`, `@deepseek-ai/dsh-llm-pi-ai`, `@earendil-works/pi-ai@0.79.3`
- Regex: `(?i)(node|engines\.node|pnpm[- ]install[- ]\-\-engine\-strict|\^22\.19\.0[- ]\|\|[- ]>=24\.0\.0|\['22\.19',[- ]24,[- ]26\]|packages/session/session\-persistence\-sqlite|import[- ]\{[- ]DatabaseSync[- ]\}[- ]from[- ]'node:sqlite|\-\-experimental\-sqlite)`

```bash
rg -n --pcre2 "(?i)(node|engines\\.node|pnpm[- ]install[- ]\\-\\-engine\\-strict|\\^22\\.19\\.0[- ]\\|\\|[- ]>=24\\.0\\.0|\\['22\\.19',[- ]24,[- ]26\\]|packages/session/session\\-persistence\\-sqlite|import[- ]\\{[- ]DatabaseSync[- ]\\}[- ]from[- ]'node:sqlite|\\-\\-experimental\\-sqlite)" source-deepseek-harness
rg -l --fixed-strings "class/process" 03-agent-context
```

## Connected notes

- **`source-link`** — [0502. Keep browser storage owned by jsdom in Vitest](0502-keep-browser-storage-owned-by-jsdom-in-vitest.md): The source note links to this decision directly.
- **`shares-code-with`** — [0098. Interrogating a draft provider endpoint](0098-interrogating-a-draft-provider-endpoint.md): Shares source implementation: `packages/llm/llm-pi-ai`, `packages/llm/llm-pi-ai/src/index.ts`.
- **`shares-code-with`** — [0329. fail-loud releases the terminal before exiting](0329-fail-loud-releases-the-terminal-before-exiting.md): Shares source implementation: `packages/llm/llm-pi-ai`, `packages/llm/llm-pi-ai/src/index.ts`.
- **`shares-code-with`** — [0623. TUI model-context resolution defers on the adapter-registration race](0623-tui-model-context-resolution-defers-on-the-adapter-registration-race.md): Shares source implementation: `packages/llm/llm-pi-ai`, `packages/llm/llm-pi-ai/src/index.ts`.
- **`shares-code-with`** — [0094. pi-ai routes are declared providers, not catalog lookups](0094-pi-ai-routes-are-declared-providers-not-catalog-lookups.md): Shares source implementation: `packages/llm/llm-pi-ai/src/index.ts`, `packages/llm/llm-pi-ai/src/invariant.ts`.
- **`shares-code-with`** — [0128. A pi-ai model declares its own input modalities, and undeclared means text](0128-a-pi-ai-model-declares-its-own-input-modalities-and-undeclared-means-tex.md): Shares source implementation: `packages/llm/llm-pi-ai/src/index.ts`, `packages/llm/llm-pi-ai/src/invariant.ts`.
- **`shares-code-with`** — [0097. Declaring a provider from the Models page](0097-declaring-a-provider-from-the-models-page.md): Shares source implementation: `packages/llm/llm-pi-ai/src/index.ts`, `packages/llm/llm-pi-ai/src/invariant.ts`.
- **`shares-code-with`** — [0087. the web configuration plane](0087-the-web-configuration-plane.md): Shares source implementation: `packages/llm/llm-pi-ai/src/index.ts`, `packages/llm/llm-pi-ai/src/invariant.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0389-raise-the-node-lts-engine-floor-to-22-19.md`.
