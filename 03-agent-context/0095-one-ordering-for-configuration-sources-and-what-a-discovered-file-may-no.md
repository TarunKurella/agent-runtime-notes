---
id: "dsh-note-0095"
title: "One ordering for configuration sources, and what a discovered file may not decide"
status: "implemented"
class: "architecture"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/architecture/2026-08-04-configuration-source-ownership.md"
implementation_evidence: "medium"
target_anchor: "sandbox capability boundary"
tags:
  - "class/architecture"
  - "concern/boundary"
  - "concern/compatibility"
  - "concern/discovery-routing"
  - "concern/evidence"
  - "concern/human-control"
  - "concern/lifecycle"
  - "concern/ownership"
  - "concern/schema-types"
  - "concern/simplification"
  - "concern/trust"
  - "domain/build-release"
  - "domain/configuration"
  - "domain/extensions"
  - "domain/filesystem"
  - "domain/llm"
  - "domain/observability"
  - "domain/security"
  - "domain/session-state"
  - "domain/shell-terminal"
  - "domain/storage"
  - "lifecycle/implemented"
  - "mechanism/policy"
aliases:
  - "loadLayeredEnv"
  - "apiKey"
  - "environment"
  - "headers"
  - "base"
  - "yaml"
  - "env"
  - "launchEnvironmentOf"
  - "$DSH_HOME/.env"
  - "process.env"
  - ".env"
  - "DEEPSEEK_BASE_URL"
  - "!!js process.env.X"
  - "--patch"
search_regex: "(?i)(loadLayeredEnv|apiKey|environment|headers|base|yaml|launchEnvironmentOf|\\$DSH_HOME/\\.env)"
---

# 0095. One ordering for configuration sources, and what a discovered file may not decide — implementation context

## Open this when

$DSH_HOME/.env had just become an ordinary environment layer, which left the harness resolving user-facing values from a flattened process.env that could no longer say where a value came from. Three consequences followed. A key stored through the web page stayed shadowed by an older key in the user's own .env, because the credential provider compared "the environment" against its file and the environment now included that file. The migration dead end the split was supposed to remove had simply moved. An endpoint could be redirected by the project.

## Source decision

One ordering for non-secret values. Every configurable value that is not itself a credential resolves in the same order; the domains differ only in which tiers exist. Settings sit above composition because that is what the settings seam does: a plugin registers its cordis entry config as the base layer and the user's section layers over it, and the seam cannot tell a value a profile's bundles set from one its user patch layer or a --patch overlay set --- all arrive as entry config.

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/architecture/2026-08-04-configuration-source-ownership.md](../02-notes/implemented/architecture/2026-08-04-configuration-source-ownership.md)
- Pinned source: [.agents/notes/implemented/architecture/2026-08-04-configuration-source-ownership.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/architecture/2026-08-04-configuration-source-ownership.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`packages/bundle/base/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/bundle/base/src/index.ts) | package entry point | Core file in the package named by the note: `packages/bundle/base`. | `named-package-member` |
| [`packages/shell/shell/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/shell/shell/src/index.ts) | package entry point | Core file in the package named by the note: `packages/shell/shell`. | `named-package-member` |
| [`packages/shell/shell/src/types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/shell/shell/src/types.ts) | public types and contract | Core file in the package named by the note: `packages/shell/shell`. | `named-package-member` |
| [`packages/bundle/base/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/bundle/base/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/bundle/base`. | `named-package-member` |
| [`packages/shell/shell/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/shell/shell/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/shell/shell`. | `named-package-member` |
| [`packages/util/launch-environment/README.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/util/launch-environment/README.md) | package contract and examples | Entry point or contract under the directory named by the note: `packages/util/launch-environment`. | `named-directory-member` |
| [`packages/util/launch-environment/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/util/launch-environment/src/index.ts) | package entry point | Entry point or contract under the directory named by the note: `packages/util/launch-environment`. Defines `launchEnvironmentOf`, a construct named by the note. | `named-directory-member, symbol-definition` |
| [`packages/util/launch-environment/package.json`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/util/launch-environment/package.json) | composition and configuration | Entry point or contract under the directory named by the note: `packages/util/launch-environment`. Contains the exact code literal `packages/util/launch-environment` named by the note. | `exact-code-occurrence, named-directory-member` |
| [`packages/util/launch-environment/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/util/launch-environment/src/invariant.ts) | runtime contract checks | Entry point or contract under the directory named by the note: `packages/util/launch-environment`. | `named-directory-member` |
| [`packages/util/launch-environment`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/packages/util/launch-environment) | package or module directory | The source note names this implementation area directly. | `named-directory` |
| [`packages/bundle/base`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/packages/bundle/base) | package or module directory | The note names this package or capability. | `named-package` |
| [`packages/shell/shell`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/packages/shell/shell) | package or module directory | The note names this package or capability. | `named-package` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `loadLayeredEnv` | `function` | [`packages/boot/app-boot/src/index.ts:177`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/boot/app-boot/src/index.ts#L177) | `export function loadLayeredEnv(` |
| `apiKey` | `const` | [`packages/e2b/e2b/src/index.ts:94`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/e2b/e2b/src/index.ts#L94) | `const apiKey = config.apiKey ?? process.env.E2B_API_KEY` |
| `environment` | `const` | [`packages/e2b/subprocess-e2b/src/terminal.ts:482`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/e2b/subprocess-e2b/src/terminal.ts#L482) | `const environment = serializeRemoteEnvironment(ambient, spec.env)` |
| `headers` | `const` | [`packages/llm/llm-deepseek/src/adapter.ts:283`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm-deepseek/src/adapter.ts#L283) | `const headers = {` |
| `base` | `const` | [`packages/lsp/lsp/src/index.ts:62`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/lsp/lsp/src/index.ts#L62) | `const base = lastSlash >= 0 ? filePath.slice(lastSlash + 1) : filePath` |
| `yaml` | `const` | [`packages/skill/skill-filesystem/src/index.ts:917`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/skill/skill-filesystem/src/index.ts#L917) | `const yaml = raw.slice(start, closing.start)` |
| `env` | `const` | [`packages/subprocess/subprocess/src/index.ts:61`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/subprocess/subprocess/src/index.ts#L61) | `const env: Record<string, string> = {}` |
| `launchEnvironmentOf` | `function` | [`packages/util/launch-environment/src/index.ts:114`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/util/launch-environment/src/index.ts#L114) | `export function launchEnvironmentOf(ctx: Context): LaunchEnvironmentSnapshot {` |

### Tests and executable evidence

- [`packages/e2b/e2b/tests/e2b.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/e2b/e2b/tests/e2b.spec.ts) — A test under the owning area exercises or imports `apiKey`.
- [`packages/e2b/e2b/tests/composition.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/e2b/e2b/tests/composition.e2e.ts) — A test under the owning area exercises or imports `apiKey`.
- [`packages/boot/app-boot/tests/app-boot.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/boot/app-boot/tests/app-boot.spec.ts) — A test under the owning area exercises or imports `loadLayeredEnv`.
- [`packages/util/launch-environment/tests/launch-environment.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/util/launch-environment/tests/launch-environment.spec.ts) — A test under the owning area exercises or imports `launchEnvironmentOf`.

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

- Tags: `class/architecture`, `concern/boundary`, `concern/compatibility`, `concern/discovery-routing`, `concern/evidence`, `concern/human-control`, `concern/lifecycle`, `concern/ownership`, `concern/schema-types`, `concern/simplification`, `concern/trust`, `domain/build-release`, `domain/configuration`, `domain/extensions`, `domain/filesystem`, `domain/llm`, `domain/observability`, `domain/security`, `domain/session-state`, `domain/shell-terminal`, `domain/storage`, `lifecycle/implemented`, `mechanism/policy`
- Aliases: `loadLayeredEnv`, `apiKey`, `environment`, `headers`, `base`, `yaml`, `env`, `launchEnvironmentOf`, `$DSH_HOME/.env`, `process.env`, `.env`, `DEEPSEEK_BASE_URL`, `!!js process.env.X`, `--patch`
- Regex: `(?i)(loadLayeredEnv|apiKey|environment|headers|base|yaml|launchEnvironmentOf|\$DSH_HOME/\.env)`

```bash
rg -n --pcre2 "(?i)(loadLayeredEnv|apiKey|environment|headers|base|yaml|launchEnvironmentOf|\\$DSH_HOME/\\.env)" source-deepseek-harness
rg -l --fixed-strings "class/architecture" 03-agent-context
```

## Connected notes

- **`source-link`** — [0073. user-settings seam (`ctx.settings`) and the file provider](0073-user-settings-seam-ctx-settings-and-the-file-provider.md): The source note links to this decision directly.
- **`source-link`** — [0096. Splitting the credential store from the user environment layer](0096-splitting-the-credential-store-from-the-user-environment-layer.md): The source note links to this decision directly.
- **`shares-code-with`** — [0287. Plugin configuration in the web settings page](0287-plugin-configuration-in-the-web-settings-page.md): Shares source implementation: `packages/bundle/base/src/index.ts`, `packages/bundle/base/src/invariant.ts`.
- **`shares-code-with`** — [0241. Windows defaults to pwsh](0241-windows-defaults-to-pwsh.md): Shares source implementation: `packages/bundle/base/src/index.ts`, `packages/bundle/base/src/invariant.ts`.
- **`shares-code-with`** — [0146. The subprocess sandbox --- confinement seam, native runners, escalation, and per-session modes](0146-the-subprocess-sandbox-confinement-seam-native-runners-escalation-and-pe.md): Shares source implementation: `packages/shell/shell/src/index.ts`, `packages/shell/shell/src/types.ts`.
- **`shares-code-with`** — [0066. The subprocess service is its own seam under the bash executors (`dsh-subprocess` / `dsh-subprocess-local`)](0066-the-subprocess-service-is-its-own-seam-under-the-bash-executors-dsh-subp.md): Shares source implementation: `packages/shell/shell/src/index.ts`, `packages/shell/shell/src/invariant.ts`.
- **`shares-code-with`** — [0009. Session persistence as an abstract service over the existing `SessionEvent`](0009-session-persistence-as-an-abstract-service-over-the-existing-sessioneven.md): Shares source implementation: `packages/shell/shell/src/index.ts`, `packages/shell/shell/src/invariant.ts`.
- **`shares-code-with`** — [0240. PowerShell executor and pwsh tool](0240-powershell-executor-and-pwsh-tool.md): Shares source implementation: `packages/shell/shell/src/index.ts`, `packages/shell/shell/src/invariant.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0095-one-ordering-for-configuration-sources-and-what-a-discovered-file-may-no.md`.
