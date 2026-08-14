---
id: "dsh-note-0235"
title: "Default Web search in shipped compositions"
status: "implemented"
class: "feature"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/feature/2026-07-31-web-default-search.md"
implementation_evidence: "medium"
target_anchor: "sandbox capability boundary"
tags:
  - "class/feature"
  - "concern/boundary"
  - "concern/cancellation-timeout"
  - "concern/compatibility"
  - "concern/discovery-routing"
  - "concern/human-control"
  - "concern/lifecycle"
  - "concern/ownership"
  - "concern/performance"
  - "concern/schema-types"
  - "concern/trust"
  - "domain/build-release"
  - "domain/configuration"
  - "domain/extensions"
  - "domain/filesystem"
  - "domain/llm"
  - "domain/security"
  - "domain/session-state"
  - "domain/shell-terminal"
  - "domain/storage"
  - "domain/testing"
  - "domain/tools"
  - "domain/ui-interaction"
  - "domain/web-retrieval"
  - "lifecycle/implemented"
  - "mechanism/adapter"
  - "mechanism/policy"
  - "mechanism/registry"
aliases:
  - "env"
  - "credentials"
  - "apiKey"
  - "DEEPSEEK_API_KEY"
  - "ctx.credentials"
  - "apps/cli/config/base.cordis.yml"
  - "dsh-web"
  - "searchProvider: deepseek-official"
  - "dsh-web-search-deepseek"
  - "dsh-tool-web"
  - "searchTimeoutMs: 60000"
  - "dsh-web-fetch-http"
  - "web_search"
  - "--config"
search_regex: "(?i)(credentials|apiKey|DEEPSEEK_API_KEY|ctx\\.credentials|apps/cli/config/base\\.cordis\\.yml|dsh\\-web|searchProvider:[- ]deepseek\\-official|dsh\\-web\\-search\\-deepseek)"
---

# 0235. Default Web search in shipped compositions — implementation context

## Open this when

The harness had a complete Web capability family---provider registry, DeepSeek/Exa/Perplexity search providers, local fetch, stable model tools, and structured result presentation---but the shipped dsh web composition mounted none of it. The model could not discover current information unless a deployment supplied a custom overlay. Merely mounting the existing DeepSeek provider would not complete the WebUI path: the Models page stores DEEPSEEK_API_KEY through ctx.credentials, while the search provider froze only the process environment at plugin load, so a key entered or rotated in the running UI would not.

## Source decision

apps/cli/config/base.cordis.yml explicitly mounts dsh-web with searchProvider: deepseek-official, dsh-web-search-deepseek, and dsh-tool-web with fetch: false and searchTimeoutMs: 60000. It does not mount dsh-web-fetch-http or select a fetch provider. The shared base makes only web_search a default for TUI, browser, and headless sessions. The explicit search provider id keeps selection independent of registration order and leaves personal or --config overlays able to replace or disable the rows.

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/feature/2026-07-31-web-default-search.md](../02-notes/implemented/feature/2026-07-31-web-default-search.md)
- Pinned source: [.agents/notes/implemented/feature/2026-07-31-web-default-search.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/feature/2026-07-31-web-default-search.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`packages/web/web/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/web/web/src/index.ts) | package entry point | Core file in the package named by the note: `packages/web/web`. | `named-package-member` |
| [`packages/web/web/src/types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/web/web/src/types.ts) | public types and contract | Core file in the package named by the note: `packages/web/web`. | `named-package-member` |
| [`packages/web/web/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/web/web/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/web/web`. | `named-package-member` |
| [`packages/web/tool-web/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/web/tool-web/src/index.ts) | package entry point | Core file in the package named by the note: `packages/web/tool-web`. | `named-package-member` |
| [`packages/web/tool-web/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/web/tool-web/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/web/tool-web`. | `named-package-member` |
| [`packages/web/web-fetch-http/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/web/web-fetch-http/src/index.ts) | package entry point | Core file in the package named by the note: `packages/web/web-fetch-http`. | `named-package-member` |
| [`packages/web/web-fetch-http/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/web/web-fetch-http/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/web/web-fetch-http`. | `named-package-member` |
| [`packages/credentials/credentials/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/credentials/credentials/src/index.ts) | package entry point | Core file in the package named by the note: `packages/credentials/credentials`. | `named-package-member` |
| [`packages/credentials/credentials/src/types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/credentials/credentials/src/types.ts) | public types and contract | Core file in the package named by the note: `packages/credentials/credentials`. | `named-package-member` |
| [`packages/web/web-search-deepseek/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/web/web-search-deepseek/src/index.ts) | package entry point | Core file in the package named by the note: `packages/web/web-search-deepseek`. Defines `credentials`, a construct named by the note. | `exact-code-occurrence, named-package-member, symbol-definition` |
| [`packages/web/web-search-deepseek/src/types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/web/web-search-deepseek/src/types.ts) | public types and contract | Core file in the package named by the note: `packages/web/web-search-deepseek`. | `named-package-member` |
| [`packages/web/web-search-deepseek/src/provider.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/web/web-search-deepseek/src/provider.ts) | provider/backend adapter | Core file in the package named by the note: `packages/web/web-search-deepseek`. Defines `apiKey`, a construct named by the note. | `exact-code-occurrence, named-package-member, symbol-definition` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `env` | `const` | [`packages/subprocess/subprocess/src/index.ts:61`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/subprocess/subprocess/src/index.ts#L61) | `const env: Record<string, string> = {}` |
| `credentials` | `const` | [`packages/web/web-search-deepseek/src/index.ts:103`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/web/web-search-deepseek/src/index.ts#L103) | `const credentials = ctx.get('credentials')` |
| `apiKey` | `const` | [`packages/web/web-search-deepseek/src/provider.ts:202`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/web/web-search-deepseek/src/provider.ts#L202) | `const apiKey = await this.apiKey(options, signal)` |

### Tests and executable evidence

- [`packages/web/web/tests/web.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/web/web/tests/web.spec.ts) — A test under the owning area exercises or imports `dsh-web`.
- [`packages/web/tool-web/tests/spill.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/web/tool-web/tests/spill.spec.ts) — A test under the owning area exercises or imports `dsh-web`. A test under the owning area exercises or imports `dsh-tool-web`.
- [`packages/web/tool-web/tests/tool-web.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/web/tool-web/tests/tool-web.spec.ts) — A test under the owning area exercises or imports `dsh-web`. A test under the owning area exercises or imports `dsh-tool-web`.
- [`packages/web/tool-web/tests/load-path.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/web/tool-web/tests/load-path.spec.ts) — A test under the owning area exercises or imports `dsh-web`. A test under the owning area exercises or imports `dsh-tool-web`.
- [`packages/web/tool-web/tests/integration.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/web/tool-web/tests/integration.spec.ts) — A test under the owning area exercises or imports `dsh-web`. A test under the owning area exercises or imports `dsh-tool-web`.
- [`packages/web/web-fetch-http/tests/fetch-http.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/web/web-fetch-http/tests/fetch-http.spec.ts) — A test under the owning area exercises or imports `dsh-web`. A test under the owning area exercises or imports `dsh-web-fetch-http`.
- [`packages/web/web-search-deepseek/tests/deepseek.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/web/web-search-deepseek/tests/deepseek.e2e.ts) — A test under the owning area exercises or imports `dsh-web-search-deepseek`. A test under the owning area exercises or imports `web_search`.
- [`packages/web/web-search-deepseek/tests/settings.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/web/web-search-deepseek/tests/settings.spec.ts) — A test under the owning area exercises or imports `dsh-web`. A test under the owning area exercises or imports `dsh-web-search-deepseek`.

## How to read the implementation

1. Start with [`packages/web/web/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/web/web/src/index.ts) because it has the strongest evidence link to the note.
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

- Tags: `class/feature`, `concern/boundary`, `concern/cancellation-timeout`, `concern/compatibility`, `concern/discovery-routing`, `concern/human-control`, `concern/lifecycle`, `concern/ownership`, `concern/performance`, `concern/schema-types`, `concern/trust`, `domain/build-release`, `domain/configuration`, `domain/extensions`, `domain/filesystem`, `domain/llm`, `domain/security`, `domain/session-state`, `domain/shell-terminal`, `domain/storage`, `domain/testing`, `domain/tools`, `domain/ui-interaction`, `domain/web-retrieval`, `lifecycle/implemented`, `mechanism/adapter`, `mechanism/policy`, `mechanism/registry`
- Aliases: `env`, `credentials`, `apiKey`, `DEEPSEEK_API_KEY`, `ctx.credentials`, `apps/cli/config/base.cordis.yml`, `dsh-web`, `searchProvider: deepseek-official`, `dsh-web-search-deepseek`, `dsh-tool-web`, `searchTimeoutMs: 60000`, `dsh-web-fetch-http`, `web_search`, `--config`
- Regex: `(?i)(credentials|apiKey|DEEPSEEK_API_KEY|ctx\.credentials|apps/cli/config/base\.cordis\.yml|dsh\-web|searchProvider:[- ]deepseek\-official|dsh\-web\-search\-deepseek)`

```bash
rg -n --pcre2 "(?i)(credentials|apiKey|DEEPSEEK_API_KEY|ctx\\.credentials|apps/cli/config/base\\.cordis\\.yml|dsh\\-web|searchProvider:[- ]deepseek\\-official|dsh\\-web\\-search\\-deepseek)" source-deepseek-harness
rg -l --fixed-strings "class/feature" 03-agent-context
```

## Connected notes

- **`source-link`** — [0229. Even out the shipped tool rosters](0229-even-out-the-shipped-tool-rosters.md): The source note links to this decision directly.
- **`shares-code-with`** — [0018. Web capability seam - stable tools over multiple providers](0018-web-capability-seam-stable-tools-over-multiple-providers.md): Shares source implementation: `packages/web/web/src/index.ts`, `packages/web/web/src/invariant.ts`.
- **`shares-code-with`** — [0655. Drop the unconsumed web observation surface --- the `providers-change` event and the status methods](0655-drop-the-unconsumed-web-observation-surface-the-providers-change-event-a.md): Shares source implementation: `packages/web/tool-web/src/index.ts`, `packages/web/tool-web/src/invariant.ts`.
- **`shares-code-with`** — [0032. Tool output spill policy](0032-tool-output-spill-policy.md): Shares source implementation: `packages/web/tool-web/src/index.ts`.
- **`shares-code-with`** — [0247. Web search source card scrolls instead of collapsing](0247-web-search-source-card-scrolls-instead-of-collapsing.md): Shares source implementation: `packages/web/tool-web/src/index.ts`, `packages/web/web/src/index.ts`.
- **`shares-code-with`** — [0287. Plugin configuration in the web settings page](0287-plugin-configuration-in-the-web-settings-page.md): Shares source implementation: `packages/web/web-search-deepseek/src/index.ts`.
- **`same-design-pressure`** — [0126. Repository naming contract and pre-release rename ledger](0126-repository-naming-contract-and-pre-release-rename-ledger.md): Shares design concerns: `concern/boundary`, `concern/cancellation-timeout`, `concern/compatibility`.
- **`same-design-pressure`** — [0500. Keyless browser e2e lane for the web GUI](0500-keyless-browser-e2e-lane-for-the-web-gui.md): Shares design concerns: `concern/boundary`, `concern/cancellation-timeout`, `concern/compatibility`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0235-default-web-search-in-shipped-compositions.md`.
