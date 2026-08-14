---
id: "dsh-note-0483"
title: "Remove the dedicated repository Plugin path"
status: "implemented"
class: "simplification"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/simplification/2026-08-09-remove-repository-plugin.md"
implementation_evidence: "medium"
target_anchor: "sandbox capability boundary"
tags:
  - "class/simplification"
  - "concern/boundary"
  - "concern/compatibility"
  - "concern/discovery-routing"
  - "concern/evidence"
  - "concern/lifecycle"
  - "concern/ownership"
  - "concern/performance"
  - "concern/schema-types"
  - "concern/simplification"
  - "domain/build-release"
  - "domain/configuration"
  - "domain/extensions"
  - "domain/filesystem"
  - "domain/llm"
  - "domain/protocols"
  - "domain/security"
  - "domain/testing"
  - "domain/web-retrieval"
  - "lifecycle/implemented"
  - "mechanism/adapter"
  - "mechanism/policy"
  - "mechanism/registry"
aliases:
  - "patch"
  - ".dsh-plugin"
  - "cordis.patch.yml"
  - "dsh plugin --profile <name> add <package-or-git-spec>"
  - "dsh.bundle.patch"
  - "@deepseek-ai/dsh-repository-plugin"
  - "dsh-plugin-prepare"
  - "repository-plugins"
  - "@cordisjs/plugin-loader/repository"
  - "@deepseek-ai/dsh-skill-filesystem"
  - "@deepseek-ai/dsh-mcp-client"
  - "PATH"
  - "dsh-skill-filesystem"
  - "dsh-mcp-client"
search_regex: "(?i)(patch|\\.dsh\\-plugin|cordis\\.patch\\.yml|dsh[- ]plugin[- ]\\-\\-profile[- ]<name>[- ]add[- ]<package\\-or\\-git\\-spec>|dsh\\.bundle\\.patch|@deepseek\\-ai/dsh\\-repository\\-plugin|dsh\\-plugin\\-prepare|repository\\-plugins)"
---

# 0483. Remove the dedicated repository Plugin path — implementation context

## Open this when

The repository Plugin path duplicated the profile bundle path for installing and composing third-party packages. It added a .dsh-plugin manifest, a generated wrapper, a preparation executable, a second Git/package cache, a Loader builtin, and repository-specific Skill and MCP adapters. Profile bundles already install npm or Git package specifications through the profile package manager, retain normal dependency and lifecycle semantics, and contribute an ordered cordis.patch.yml layer that can mount ordinary Cordis Plugins. The duplicate path also exposed less configuration than a bundle.

## Source decision

DeepSeek Harness has one standalone external-Plugin distribution path: installable profile bundles. dsh plugin --profile add records the dependency in the profile package, and the installed package declares dsh.bundle.patch to contribute its patch layer. The package manager owns source acquisition, versions, dependencies, build lifecycles, and its lockfile. The bundle patch owns Cordis Plugin selection and complete Plugin config.

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/simplification/2026-08-09-remove-repository-plugin.md](../02-notes/implemented/simplification/2026-08-09-remove-repository-plugin.md)
- Pinned source: [.agents/notes/implemented/simplification/2026-08-09-remove-repository-plugin.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/simplification/2026-08-09-remove-repository-plugin.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`packages/mcp/mcp-client/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/mcp/mcp-client/src/index.ts) | package entry point | Core file in the package named by the note: `packages/mcp/mcp-client`. | `named-package-member` |
| [`packages/mcp/mcp-client/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/mcp/mcp-client/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/mcp/mcp-client`. | `named-package-member` |
| [`packages/skill/skill-filesystem/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/skill/skill-filesystem/src/index.ts) | package entry point | Core file in the package named by the note: `packages/skill/skill-filesystem`. | `named-package-member` |
| [`packages/skill/skill-filesystem/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/skill/skill-filesystem/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/skill/skill-filesystem`. | `named-package-member` |
| [`packages/mcp/mcp-client`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/packages/mcp/mcp-client) | package or module directory | The note names this package or capability. | `named-package` |
| [`packages/skill/skill-filesystem`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/packages/skill/skill-filesystem) | package or module directory | The note names this package or capability. | `named-package` |
| [`packages/fs/tool-fs/src/diff.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/tool-fs/src/diff.ts) | runtime implementation | Defines `patch`, a construct named by the note. | `symbol-definition` |
| [`packages/mcp/mcp-client/README.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/mcp/mcp-client/README.md) | package contract and examples | Core file in the package named by the note: `packages/mcp/mcp-client`. | `named-package-member` |
| [`packages/mcp/mcp-client/package.json`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/mcp/mcp-client/package.json) | composition and configuration | Core file in the package named by the note: `packages/mcp/mcp-client`. | `named-package-member` |
| [`packages/skill/skill-filesystem/README.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/skill/skill-filesystem/README.md) | package contract and examples | Core file in the package named by the note: `packages/skill/skill-filesystem`. | `named-package-member` |
| [`packages/skill/skill-filesystem/package.json`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/skill/skill-filesystem/package.json) | composition and configuration | Core file in the package named by the note: `packages/skill/skill-filesystem`. | `named-package-member` |
| [`pnpm-lock.yaml`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/pnpm-lock.yaml) | composition and configuration | Contains the exact code literal `dsh-skill-filesystem` named by the note. Contains the exact code literal `dsh-mcp-client` named by the note. | `exact-code-occurrence` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `patch` | `const` | [`packages/fs/tool-fs/src/diff.ts:34`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/tool-fs/src/diff.ts#L34) | `const patch = structuredPatch('', '', before, after, undefined, undefined, { context: DIFF_CONTEXT })` |

### Tests and executable evidence

- [`packages/mcp/mcp-client/tests/apply.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/mcp/mcp-client/tests/apply.spec.ts) — A test under the owning area exercises or imports `dsh-mcp-client`.
- [`packages/mcp/mcp-client/tests/load-path.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/mcp/mcp-client/tests/load-path.spec.ts) — A test under the owning area exercises or imports `dsh-mcp-client`.
- [`packages/mcp/mcp-client/tests/fixture-server.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/mcp/mcp-client/tests/fixture-server.ts) — A test under the owning area exercises or imports `dsh-mcp-client`.
- [`packages/mcp/mcp-client/tests/mcp-client.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/mcp/mcp-client/tests/mcp-client.e2e.ts) — A test under the owning area exercises or imports `dsh-mcp-client`.
- [`packages/mcp/mcp-client/tests/reconnect.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/mcp/mcp-client/tests/reconnect.spec.ts) — A test under the owning area exercises or imports `dsh-mcp-client`.
- [`packages/mcp/mcp-client/tests/mcp-client.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/mcp/mcp-client/tests/mcp-client.spec.ts) — A test under the owning area exercises or imports `dsh-mcp-client`.
- [`packages/skill/skill-filesystem/tests/skill-filesystem.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/skill/skill-filesystem/tests/skill-filesystem.spec.ts) — A test under the owning area exercises or imports `dsh-skill-filesystem`.
- [`scripts/gen-third-party-notices.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/gen-third-party-notices.spec.ts) — Contains the exact code literal `dsh-mcp-client` named by the note.
- Source verification intent: Static gates reject stale package, config, documentation, graph, and workspace references. The existing dsh plugin built-CLI acceptance covers profile initialization, package-manager installation, bundle discovery, and layer reconciliation. Declarative package-relative Skill and MCP bundle resources remain a named coverage gap in this removal layer.

## How to read the implementation

1. Start with [`packages/mcp/mcp-client/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/mcp/mcp-client/src/index.ts) because it has the strongest evidence link to the note.
2. Read public types and entry points before implementation details. Write down the contract in plain language.
3. Follow the named constructs into their callers and tests. Confirm which component can actually enforce the rule.
4. Reconstruct one failure path: cancellation, crash, retry, stale state, partial write, or incompatible provider input—whichever the note is protecting against.
5. Only then map the contract into the current Rust repository. Target paths must come from that repository, never from guesswork in this wiki.

## Rust harness port

**Likely integration point:** sandbox capability boundary.

Preserve the behavior and ownership rule. Do not mechanically translate Cordis, TypeScript declaration merging, Node streams, or package topology into Rust.

Relevant standing rules:

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

- Tags: `class/simplification`, `concern/boundary`, `concern/compatibility`, `concern/discovery-routing`, `concern/evidence`, `concern/lifecycle`, `concern/ownership`, `concern/performance`, `concern/schema-types`, `concern/simplification`, `domain/build-release`, `domain/configuration`, `domain/extensions`, `domain/filesystem`, `domain/llm`, `domain/protocols`, `domain/security`, `domain/testing`, `domain/web-retrieval`, `lifecycle/implemented`, `mechanism/adapter`, `mechanism/policy`, `mechanism/registry`
- Aliases: `patch`, `.dsh-plugin`, `cordis.patch.yml`, `dsh plugin --profile <name> add <package-or-git-spec>`, `dsh.bundle.patch`, `@deepseek-ai/dsh-repository-plugin`, `dsh-plugin-prepare`, `repository-plugins`, `@cordisjs/plugin-loader/repository`, `@deepseek-ai/dsh-skill-filesystem`, `@deepseek-ai/dsh-mcp-client`, `PATH`, `dsh-skill-filesystem`, `dsh-mcp-client`
- Regex: `(?i)(patch|\.dsh\-plugin|cordis\.patch\.yml|dsh[- ]plugin[- ]\-\-profile[- ]<name>[- ]add[- ]<package\-or\-git\-spec>|dsh\.bundle\.patch|@deepseek\-ai/dsh\-repository\-plugin|dsh\-plugin\-prepare|repository\-plugins)`

```bash
rg -n --pcre2 "(?i)(patch|\\.dsh\\-plugin|cordis\\.patch\\.yml|dsh[- ]plugin[- ]\\-\\-profile[- ]<name>[- ]add[- ]<package\\-or\\-git\\-spec>|dsh\\.bundle\\.patch|@deepseek\\-ai/dsh\\-repository\\-plugin|dsh\\-plugin\\-prepare|repository\\-plugins)" source-deepseek-harness
rg -l --fixed-strings "class/simplification" 03-agent-context
```

## Connected notes

- **`shares-code-with`** — [0234. Third-party memory MCP examples](0234-third-party-memory-mcp-examples.md): Shares source implementation: `packages/mcp/mcp-client`, `packages/mcp/mcp-client/README.md`.
- **`shares-code-with`** — [0147. MCP client plugin --- connect to external MCP servers and bridge their tools](0147-mcp-client-plugin-connect-to-external-mcp-servers-and-bridge-their-tools.md): Shares source implementation: `packages/mcp/mcp-client/src/index.ts`, `packages/mcp/mcp-client/src/invariant.ts`.
- **`shares-code-with`** — [0262. Bundled dsh badge skill](0262-bundled-dsh-badge-skill.md): Shares source implementation: `packages/skill/skill-filesystem`, `packages/skill/skill-filesystem/src/index.ts`.
- **`shares-code-with`** — [0564. Bash-backed grep and glob discovery tools](0564-bash-backed-grep-and-glob-discovery-tools.md): Shares source implementation: `packages/fs/tool-fs/src/diff.ts`.
- **`shares-code-with`** — [0192. Skill catalog hot refresh](0192-skill-catalog-hot-refresh.md): Shares source implementation: `packages/skill/skill-filesystem/src/index.ts`, `packages/skill/skill-filesystem/src/invariant.ts`.
- **`same-design-pressure`** — [0126. Repository naming contract and pre-release rename ledger](0126-repository-naming-contract-and-pre-release-rename-ledger.md): Shares design concerns: `concern/boundary`, `concern/compatibility`, `concern/discovery-routing`.
- **`shares-code-with`** — [0116. The skill registry is host-held and layered per scope](0116-the-skill-registry-is-host-held-and-layered-per-scope.md): Shares source implementation: `packages/skill/skill-filesystem/src/index.ts`, `packages/skill/skill-filesystem/src/invariant.ts`.
- **`same-design-pressure`** — [0500. Keyless browser e2e lane for the web GUI](0500-keyless-browser-e2e-lane-for-the-web-gui.md): Shares design concerns: `concern/boundary`, `concern/compatibility`, `concern/discovery-routing`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0483-remove-the-dedicated-repository-plugin-path.md`.
