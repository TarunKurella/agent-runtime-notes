---
id: "dsh-note-0287"
title: "Plugin configuration in the web settings page"
status: "implemented"
class: "feature"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/feature/2026-08-10-web-plugin-configuration.md"
implementation_evidence: "medium"
target_anchor: "sandbox capability boundary"
tags:
  - "class/feature"
  - "concern/boundary"
  - "concern/cancellation-timeout"
  - "concern/discovery-routing"
  - "concern/evidence"
  - "concern/lifecycle"
  - "concern/ownership"
  - "concern/schema-types"
  - "concern/trust"
  - "domain/agent-loop"
  - "domain/build-release"
  - "domain/configuration"
  - "domain/context"
  - "domain/extensions"
  - "domain/llm"
  - "domain/multi-agent"
  - "domain/observability"
  - "domain/security"
  - "domain/session-state"
  - "domain/shell-terminal"
  - "domain/storage"
  - "domain/ui-interaction"
  - "domain/web-retrieval"
  - "lifecycle/implemented"
  - "mechanism/generation"
  - "mechanism/policy"
  - "mechanism/registry"
aliases:
  - "agents"
  - "user"
  - "search"
  - "shell"
  - "maxParallelToolCalls"
  - "describe"
  - "WEB_SETTINGS_NAMESPACES"
  - "base"
  - "client"
  - "SHELL_SETTINGS_NAMESPACE"
  - "yaml"
  - "unset"
  - "config"
  - "item"
search_regex: "(?i)(agents|user|search|shell|maxParallelToolCalls|describe|WEB_SETTINGS_NAMESPACES|base)"
---

# 0287. Plugin configuration in the web settings page — implementation context

## Open this when

Everything a plugin can be configured with lived in cordis.yml. A user who wanted a longer shell timeout, a different search endpoint, or fewer parallel tool calls had to find the composition file, know its shape, and restart --- while the Models page had shown for months that a settings namespace can be edited from the browser and take effect immediately. The seam that made the Models page possible was already general: any plugin may register a namespace, and settings.describe serves its schema, its layers, and its revision. What was missing was on the two ends.

## Source decision

Three host-plane plugins register their own settings namespace, and one browser-side Plugins section aggregates feature-owned tabs. Its configurable tab renders whatever editable settings the deployment exposes. Layering, unchanged. A section resolves as schema defaults → the plugin's composition entry → the user layer. Each plugin passes its cordis.yml entry as the base and reads its config through a source thunk, so a stored change reaches the next use and a detaching settings provider leaves the composition entry running.

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/feature/2026-08-10-web-plugin-configuration.md](../02-notes/implemented/feature/2026-08-10-web-plugin-configuration.md)
- Pinned source: [.agents/notes/implemented/feature/2026-08-10-web-plugin-configuration.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/feature/2026-08-10-web-plugin-configuration.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`packages/bundle/base/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/bundle/base/src/index.ts) | package entry point | Core file in the package named by the note: `packages/bundle/base`. | `named-package-member` |
| [`packages/shell/shell/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/shell/shell/src/index.ts) | package entry point | Core file in the package named by the note: `packages/shell/shell`. Defines `SHELL_SETTINGS_NAMESPACE`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/shell/shell/src/types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/shell/shell/src/types.ts) | public types and contract | Core file in the package named by the note: `packages/shell/shell`. | `named-package-member` |
| [`packages/bundle/base/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/bundle/base/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/bundle/base`. | `named-package-member` |
| [`packages/core/agent-loop/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent-loop/src/index.ts) | package entry point | Core file in the package named by the note: `packages/core/agent-loop`. Defines `maxParallelToolCalls`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/shell/shell/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/shell/shell/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/shell/shell`. | `named-package-member` |
| [`packages/core/agent-loop/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent-loop/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/core/agent-loop`. | `named-package-member` |
| [`packages/web/web-search-deepseek/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/web/web-search-deepseek/src/index.ts) | package entry point | Core file in the package named by the note: `packages/web/web-search-deepseek`. | `named-package-member` |
| [`packages/web/web-search-deepseek/src/types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/web/web-search-deepseek/src/types.ts) | public types and contract | Core file in the package named by the note: `packages/web/web-search-deepseek`. | `named-package-member` |
| [`packages/client/ui-settings-plugins/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-settings-plugins/src/index.ts) | package entry point | Core file in the package named by the note: `packages/client/ui-settings-plugins`. | `named-package-member` |
| [`packages/web/web-search-deepseek/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/web/web-search-deepseek/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/web/web-search-deepseek`. | `named-package-member` |
| [`packages/client/ui-settings-plugins/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-settings-plugins/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/client/ui-settings-plugins`. | `named-package-member` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `agents` | `const` | [`packages/acp/acp/src/index.ts:108`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/acp/acp/src/index.ts#L108) | `const agents = ctx.agents` |
| `user` | `const` | [`packages/client/ui-settings-plugins/src/client/card-form.ts:344`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-settings-plugins/src/client/card-form.ts#L344) | `const user = this.userLayer()` |
| `search` | `const` | [`packages/client/ui-tool/src/client/tool/ToolDetails.tsx:41`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-tool/src/client/tool/ToolDetails.tsx#L41) | `const search = searchCardModel(block)` |
| `shell` | `const` | [`packages/client/web/src/boot.tsx:121`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/web/src/boot.tsx#L121) | `const shell = this.ctx.get('appShell')` |
| `maxParallelToolCalls` | `const` | [`packages/core/agent-loop/src/index.ts:134`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent-loop/src/index.ts#L134) | `const maxParallelToolCalls = value ?? DEFAULT_MAX_PARALLEL_TOOL_CALLS` |
| `describe` | `function` | [`packages/core/tools/src/py-types.ts:223`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/py-types.ts#L223) | `function describe(schema: object): string \| undefined {` |
| `WEB_SETTINGS_NAMESPACES` | `const` | [`packages/host/apiproxy/src/api-proxy.ts:126`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/src/api-proxy.ts#L126) | `const WEB_SETTINGS_NAMESPACES = [` |
| `base` | `const` | [`packages/lsp/lsp/src/index.ts:62`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/lsp/lsp/src/index.ts#L62) | `const base = lastSlash >= 0 ? filePath.slice(lastSlash + 1) : filePath` |
| `client` | `const` | [`packages/sdk/client/src/api.ts:148`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/sdk/client/src/api.ts#L148) | `const client = this.harness.client` |
| `SHELL_SETTINGS_NAMESPACE` | `const` | [`packages/shell/shell/src/index.ts:22`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/shell/shell/src/index.ts#L22) | `export const SHELL_SETTINGS_NAMESPACE = settingsNamespace('shell')` |
| `yaml` | `const` | [`packages/skill/skill-filesystem/src/index.ts:917`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/skill/skill-filesystem/src/index.ts#L917) | `const yaml = raw.slice(start, closing.start)` |
| `unset` | `const` | [`packages/test-support/client-runtime/src/settings-scope.ts:35`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/test-support/client-runtime/src/settings-scope.ts#L35) | `const unset = vi.fn(() => Promise.resolve())` |
| `config` | `const` | [`vendor/loader/src/index.ts:93`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/loader/src/index.ts#L93) | `const config = next()` |
| `item` | `const` | [`vendor/schemastery/src/index.ts:414`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/schemastery/src/index.ts#L414) | `const item = schema?.simplify(value[key])` |

### Tests and executable evidence

- [`packages/typert/generator/tests/fixtures/type-model/packages/client/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/typert/generator/tests/fixtures/type-model/packages/client/src/index.ts) — Core file in the package named by the note: `packages/typert/generator/tests/fixtures/type-model/packages/client`.
- [`packages/typert/generator/tests/fixtures/type-model/packages/client`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/typert/generator/tests/fixtures/type-model/packages/client) — The note names this package or capability.
- [`packages/typert/generator/tests/fixtures/type-model/packages/client/package.json`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/typert/generator/tests/fixtures/type-model/packages/client/package.json) — Core file in the package named by the note: `packages/typert/generator/tests/fixtures/type-model/packages/client`.
- [`packages/core/agent-loop/tests/loop.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent-loop/tests/loop.spec.ts) — A test under the owning area exercises or imports `unset`.
- [`packages/core/agent-loop/tests/settings.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent-loop/tests/settings.spec.ts) — A test under the owning area exercises or imports `maxParallelToolCalls`.
- [`packages/core/agent-loop/tests/tool-calls.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent-loop/tests/tool-calls.spec.ts) — A test under the owning area exercises or imports `maxParallelToolCalls`.
- [`packages/web/web-search-deepseek/tests/settings.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/web/web-search-deepseek/tests/settings.spec.ts) — A test under the owning area exercises or imports `web-search-deepseek`.
- [`packages/web/web-search-deepseek/tests/deepseek.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/web/web-search-deepseek/tests/deepseek.spec.ts) — A test under the owning area exercises or imports `web-search-deepseek`.

## How to read the implementation

1. Start with [`packages/bundle/base/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/bundle/base/src/index.ts) because it has the strongest evidence link to the note.
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

### Smallest safe implementation slice

1. Express the semantic contract as Rust types and one owning boundary.
2. Add the smallest adapter or service needed by the existing loop; do not add a new model-facing tool unless the four-tool surface cannot express the job.
3. Record durable evidence before deriving summaries or UI state.
4. Add one happy-path test, one interruption test, and one replay/restart test where state survives a turn.
5. Keep the patch reversible until the source note's strongest failure case is proven in the Rust harness.

## Search handles

- Tags: `class/feature`, `concern/boundary`, `concern/cancellation-timeout`, `concern/discovery-routing`, `concern/evidence`, `concern/lifecycle`, `concern/ownership`, `concern/schema-types`, `concern/trust`, `domain/agent-loop`, `domain/build-release`, `domain/configuration`, `domain/context`, `domain/extensions`, `domain/llm`, `domain/multi-agent`, `domain/observability`, `domain/security`, `domain/session-state`, `domain/shell-terminal`, `domain/storage`, `domain/ui-interaction`, `domain/web-retrieval`, `lifecycle/implemented`, `mechanism/generation`, `mechanism/policy`, `mechanism/registry`
- Aliases: `agents`, `user`, `search`, `shell`, `maxParallelToolCalls`, `describe`, `WEB_SETTINGS_NAMESPACES`, `base`, `client`, `SHELL_SETTINGS_NAMESPACE`, `yaml`, `unset`, `config`, `item`
- Regex: `(?i)(agents|user|search|shell|maxParallelToolCalls|describe|WEB_SETTINGS_NAMESPACES|base)`

```bash
rg -n --pcre2 "(?i)(agents|user|search|shell|maxParallelToolCalls|describe|WEB_SETTINGS_NAMESPACES|base)" source-deepseek-harness
rg -l --fixed-strings "class/feature" 03-agent-context
```

## Connected notes

- **`source-link`** — [0087. the web configuration plane](0087-the-web-configuration-plane.md): The source note links to this decision directly.
- **`shares-code-with`** — [0095. One ordering for configuration sources, and what a discovered file may not decide](0095-one-ordering-for-configuration-sources-and-what-a-discovered-file-may-no.md): Shares source implementation: `packages/bundle/base/src/index.ts`, `packages/bundle/base/src/invariant.ts`.
- **`shares-code-with`** — [0241. Windows defaults to pwsh](0241-windows-defaults-to-pwsh.md): Shares source implementation: `packages/bundle/base/src/index.ts`, `packages/bundle/base/src/invariant.ts`.
- **`shares-code-with`** — [0146. The subprocess sandbox --- confinement seam, native runners, escalation, and per-session modes](0146-the-subprocess-sandbox-confinement-seam-native-runners-escalation-and-pe.md): Shares source implementation: `packages/core/agent-loop/src/index.ts`, `packages/shell/shell/src/index.ts`.
- **`shares-code-with`** — [0066. The subprocess service is its own seam under the bash executors (`dsh-subprocess` / `dsh-subprocess-local`)](0066-the-subprocess-service-is-its-own-seam-under-the-bash-executors-dsh-subp.md): Shares source implementation: `packages/shell/shell/src/index.ts`, `packages/shell/shell/src/invariant.ts`.
- **`shares-code-with`** — [0193. tmux-location context](0193-tmux-location-context.md): Shares source implementation: `packages/shell/shell/src/index.ts`, `packages/shell/shell/src/invariant.ts`.
- **`shares-code-with`** — [0139. dsh-hook-protocol --- the shared Claude Code / Codex hook wire-protocol core](0139-dsh-hook-protocol-the-shared-claude-code-codex-hook-wire-protocol-core.md): Shares source implementation: `packages/shell/shell/src/index.ts`, `packages/shell/shell/src/invariant.ts`.
- **`shares-code-with`** — [0009. Session persistence as an abstract service over the existing `SessionEvent`](0009-session-persistence-as-an-abstract-service-over-the-existing-sessioneven.md): Shares source implementation: `packages/shell/shell/src/index.ts`, `packages/shell/shell/src/invariant.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0287-plugin-configuration-in-the-web-settings-page.md`.
