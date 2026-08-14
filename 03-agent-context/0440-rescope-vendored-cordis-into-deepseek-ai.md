---
id: "dsh-note-0440"
title: "Rescope vendored Cordis into @deepseek-ai"
status: "implemented"
class: "process"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/process/2026-08-10-vendor-package-rescope.md"
implementation_evidence: "high"
target_anchor: "repository tests and engineering policy"
tags:
  - "class/process"
  - "concern/boundary"
  - "concern/compatibility"
  - "concern/discovery-routing"
  - "concern/evidence"
  - "domain/build-release"
  - "domain/configuration"
  - "domain/extensions"
  - "domain/filesystem"
  - "domain/session-state"
  - "domain/storage"
  - "lifecycle/implemented"
  - "mechanism/event-sourcing"
  - "mechanism/policy"
  - "mechanism/registry"
aliases:
  - "json"
  - "paths"
  - "yaml"
  - "GROUP_ORDER"
  - "vendor/"
  - "@cordisjs/plugin-*"
  - "@deepseek-ai/dsh-*"
  - "@deepseek-ai"
  - "vendor/README.md"
  - "cordis/"
  - "@deepseek-ai/cordis"
  - "cosmokit/"
  - "@deepseek-ai/cosmokit"
  - "schemastery/"
search_regex: "(?i)(json|paths|yaml|GROUP_ORDER|vendor/|@cordisjs/plugin\\-\\*|@deepseek\\-ai/dsh\\-\\*|@deepseek\\-ai)"
---

# 0440. Rescope vendored Cordis into @deepseek-ai — implementation context

## Open this when

The nine packages under vendor/ kept their upstream npm names (cordis, cosmokit, schemastery, @cordisjs/plugin-). That premise does not survive publication: every harness package declares cordis as a peer dependency, so a consumer installing @deepseek-ai/dsh- must resolve it from the registry, which means publishing the harness publishes this framework layer too. Publishing it under the upstream names squats them on the registry, and where that registry proxies npmjs, the same-name entries shadow the real upstream packages and install the wrong framework into unrelated projects.

## Source decision

All nine packages move into the @deepseek-ai scope. Directory names, upstream version numbers, and dependency ranges stay untouched, so the vendor/README.md manifest still reads as an upstream snapshot. docs/rescope.md restates this mapping for consumers. The rewrite touches only delimited, complete package-name tokens: quoted or backticked specifiers (optionally with a /subpath), package.json names and dependency keys, cordis.yml name: values, and tsconfig.base.json paths keys.

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/process/2026-08-10-vendor-package-rescope.md](../02-notes/implemented/process/2026-08-10-vendor-package-rescope.md)
- Pinned source: [.agents/notes/implemented/process/2026-08-10-vendor-package-rescope.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/process/2026-08-10-vendor-package-rescope.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`docs/rescope.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/rescope.md) | package contract and examples | The source note names this file directly. Contains the exact code literal `scripts/rescope-vendor.ts` named by the note. | `exact-code-occurrence, named-file` |
| [`vendor/README.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/README.md) | package contract and examples | The source note names this file directly. Contains the exact code literal `docs/rescope.md` named by the note. | `exact-code-occurrence, named-file` |
| [`scripts/gen-doc-graphs.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/gen-doc-graphs.ts) | repository automation | The source note names this file directly. Defines `GROUP_ORDER`, a construct named by the note. | `exact-code-occurrence, named-file, symbol-definition` |
| [`scripts/rescope-vendor.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/rescope-vendor.ts) | repository automation | The source note names this file directly. Contains the exact code literal `vendor/README.md` named by the note. | `exact-code-occurrence, named-file` |
| [`scripts/gen-module-graph.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/gen-module-graph.ts) | repository automation | The source note names this file directly. Contains the exact code literal `scripts/gen-module-graph.ts` named by the note. | `exact-code-occurrence, named-file` |
| [`vendor/loader/src/config/tree.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/loader/src/config/tree.ts) | runtime implementation | The source note names this file directly. | `named-file` |
| [`vendor/hmr/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/hmr/src/index.ts) | package entry point | Core file in the package named by the note: `vendor/hmr`. | `named-package-member` |
| [`vendor/group/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/group/src/index.ts) | package entry point | Core file in the package named by the note: `vendor/group`. | `named-package-member` |
| [`vendor/timer/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/timer/src/index.ts) | package entry point | Core file in the package named by the note: `vendor/timer`. | `named-package-member` |
| [`vendor/cordis/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/cordis/src/index.ts) | package entry point | Core file in the package named by the note: `vendor/cordis`. | `named-package-member` |
| [`vendor/loader/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/loader/src/index.ts) | package entry point | Core file in the package named by the note: `vendor/loader`. | `named-package-member` |
| [`vendor/cordis/src/logger.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/cordis/src/logger.ts) | runtime implementation | Core file in the package named by the note: `vendor/cordis`. | `named-package-member` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `json` | `const` | [`packages/client/modules/src/index.ts:169`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/modules/src/index.ts#L169) | `const json = JSON.stringify(graph).replaceAll('<', '\\u003c')` |
| `paths` | `const` | [`packages/shell/tool-bash/src/render.ts:82`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/shell/tool-bash/src/render.ts#L82) | `const paths = [read.stdoutSpillPath, read.stderrSpillPath].filter((path): path is string => path !== undefined)` |
| `yaml` | `const` | [`packages/skill/skill-filesystem/src/index.ts:917`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/skill/skill-filesystem/src/index.ts#L917) | `const yaml = raw.slice(start, closing.start)` |
| `GROUP_ORDER` | `const` | [`scripts/gen-doc-graphs.ts:63`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/gen-doc-graphs.ts#L63) | `const GROUP_ORDER = [` |

### Tests and executable evidence

- [`scripts/gen-third-party-notices.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/gen-third-party-notices.spec.ts) — Contains the exact code literal `vendor/README.md` named by the note.

## How to read the implementation

1. Start with [`docs/rescope.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/rescope.md) because it has the strongest evidence link to the note.
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
- **`rust/quickjs-boundary`** — QuickJS holds orchestration variables; Rust/Tokio owns async execution and exposes only explicitly granted capabilities.
- **`rust/trait-seam`** — Use a small trait at the capability boundary; keep policy and model-facing schemas outside the backend implementation.

### Smallest safe implementation slice

1. Express the semantic contract as Rust types and one owning boundary.
2. Add the smallest adapter or service needed by the existing loop; do not add a new model-facing tool unless the four-tool surface cannot express the job.
3. Record durable evidence before deriving summaries or UI state.
4. Add one happy-path test, one interruption test, and one replay/restart test where state survives a turn.
5. Keep the patch reversible until the source note's strongest failure case is proven in the Rust harness.

## Search handles

- Tags: `class/process`, `concern/boundary`, `concern/compatibility`, `concern/discovery-routing`, `concern/evidence`, `domain/build-release`, `domain/configuration`, `domain/extensions`, `domain/filesystem`, `domain/session-state`, `domain/storage`, `lifecycle/implemented`, `mechanism/event-sourcing`, `mechanism/policy`, `mechanism/registry`
- Aliases: `json`, `paths`, `yaml`, `GROUP_ORDER`, `vendor/`, `@cordisjs/plugin-*`, `@deepseek-ai/dsh-*`, `@deepseek-ai`, `vendor/README.md`, `cordis/`, `@deepseek-ai/cordis`, `cosmokit/`, `@deepseek-ai/cosmokit`, `schemastery/`
- Regex: `(?i)(json|paths|yaml|GROUP_ORDER|vendor/|@cordisjs/plugin\-\*|@deepseek\-ai/dsh\-\*|@deepseek\-ai)`

```bash
rg -n --pcre2 "(?i)(json|paths|yaml|GROUP_ORDER|vendor/|@cordisjs/plugin\\-\\*|@deepseek\\-ai/dsh\\-\\*|@deepseek\\-ai)" source-deepseek-harness
rg -l --fixed-strings "class/process" 03-agent-context
```

## Connected notes

- **`shares-code-with`** — [0439. Private npm publication as three independent sequences](0439-private-npm-publication-as-three-independent-sequences.md): Shares source implementation: `vendor/README.md`, `vendor/cordis/src/index.ts`.
- **`shares-code-with`** — [0514. Cordis Host/Client Dynamic Plugin Runtime](0514-cordis-host-client-dynamic-plugin-runtime.md): Shares source implementation: `vendor/cordis/src/index.ts`, `vendor/timer/src/index.ts`.
- **`shares-code-with`** — [0127. Trajectory assembly from registered Conversation Contexts](0127-trajectory-assembly-from-registered-conversation-contexts.md): Shares source implementation: `vendor/hmr/src/index.ts`, `vendor/loader/src/config/tree.ts`.
- **`shares-code-with`** — [0052. The slot system standard --- single register, four props shares, and the framework store seat](0052-the-slot-system-standard-single-register-four-props-shares-and-the-frame.md): Shares source implementation: `scripts/rescope-vendor.ts`, `vendor/hmr/src/index.ts`.
- **`shares-code-with`** — [0546. Extract example apps into packages](0546-extract-example-apps-into-packages.md): Shares source implementation: `vendor/hmr/src/index.ts`, `vendor/timer/src/index.ts`.
- **`shares-code-with`** — [0093. A session's agent is composed from a preset cordis.yml](0093-a-session-s-agent-is-composed-from-a-preset-cordis-yml.md): Shares source implementation: `vendor/cordis/src/index.ts`.
- **`shares-code-with`** — [0378. Vendor Cordis as source, not npm dependencies](0378-vendor-cordis-as-source-not-npm-dependencies.md): Shares source implementation: `scripts/rescope-vendor.ts`, `vendor/README.md`.
- **`shares-code-with`** — [0561. Plan mode --- a logged per-agent session mode](0561-plan-mode-a-logged-per-agent-session-mode.md): Shares source implementation: `vendor/cordis/src/index.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0440-rescope-vendored-cordis-into-deepseek-ai.md`.
