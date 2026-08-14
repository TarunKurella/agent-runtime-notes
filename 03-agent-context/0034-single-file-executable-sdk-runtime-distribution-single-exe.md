---
id: "dsh-note-0034"
title: "Single-file executable SDK runtime distribution (single-exe)"
status: "implemented"
class: "architecture"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/architecture/2026-07-10-single-file-executable-sdk-runtime-distribution.md"
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
  - "domain/multi-agent"
  - "domain/protocols"
  - "domain/session-state"
  - "domain/shell-terminal"
  - "domain/storage"
  - "domain/testing"
  - "lifecycle/implemented"
  - "mechanism/event-sourcing"
  - "mechanism/projection"
  - "mechanism/registry"
  - "mechanism/streaming"
aliases:
  - "shutdown"
  - "loadEnv"
  - "installFailLoud"
  - "boot"
  - "json"
  - "node"
  - "baseUrl"
  - "exit"
  - "HarnessSdkJsonRpcServer"
  - "files"
  - "resolve_bundled_launch_args"
  - "bin"
  - "url"
  - "cordis.yml"
search_regex: "(?i)(shutdown|loadEnv|installFailLoud|boot|json|node|baseUrl|exit)"
---

# 0034. Single-file executable SDK runtime distribution (single-exe) — implementation context

## Open this when

DeepSeek Harness needs a dedicated SDK distribution form for the Python library --- no Node installation, runs directly on the target platform: a single-file executable (hereafter "the exe") that exposes a stdio JSON-RPC serving interface (HarnessSdkJsonRpcServer, the Python SDK's peer), where the plugins and configuration actually booted are decided entirely by a cordis.yml supplied from outside the exe.

## Source decision

The exe is packaged with the --sea (enhanced SEA) mode of @yao-pkg/pkg (the actively maintained fork after vercel/pkg was archived). Relative to Node's native SEA, pkg adds a /snapshot VFS and runtime module hooks on top, hands the ESM entry to Node's default ESM loader unchanged, and depends on no ESM→CJS transpilation. > Measured (macos-arm64, node24 target, pkg 6.21.0): bare-specifier ESM dynamic import inside the VFS (including top-level await), CJS interop, node:sqlite, fail-loud on package names outside the set, and on-disk ESM import outside the VFS all pass; import.meta.url comes through unchanged.

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/architecture/2026-07-10-single-file-executable-sdk-runtime-distribution.md](../02-notes/implemented/architecture/2026-07-10-single-file-executable-sdk-runtime-distribution.md)
- Pinned source: [.agents/notes/implemented/architecture/2026-07-10-single-file-executable-sdk-runtime-distribution.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/architecture/2026-07-10-single-file-executable-sdk-runtime-distribution.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`.gitlab-ci.yml`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.gitlab-ci.yml) | composition and configuration | The source note names this file directly. Contains the exact code literal `scripts/build-exe-for-python-sdk.ts` named by the note. | `exact-code-occurrence, named-file` |
| [`python/README.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/python/README.md) | package contract and examples | The source note names this file directly. | `named-file` |
| [`pnpm-workspace.yaml`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/pnpm-workspace.yaml) | composition and configuration | The source note names this file directly. Contains the exact code literal `python/sdk-runtime` named by the note. | `exact-code-occurrence, named-file` |
| [`packages/sdk/server/README.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/sdk/server/README.md) | package contract and examples | Entry point or contract under the directory named by the note: `packages/sdk/server`. The source note names this file directly. | `named-directory-member, named-file, named-package-member` |
| [`python/sdk-runtime/package.json`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/python/sdk-runtime/package.json) | composition and configuration | Entry point or contract under the directory named by the note: `python/sdk-runtime`. The source note names this file directly. | `exact-code-occurrence, named-directory-member, named-file, named-package-member` |
| [`scripts/build-python-release.py`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/build-python-release.py) | repository automation | The source note names this file directly. | `named-file` |
| [`scripts/verify-runtime-closure.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/verify-runtime-closure.ts) | repository automation | The source note names this file directly. Contains the exact code literal `python/sdk-runtime/package.json` named by the note. | `exact-code-occurrence, named-file` |
| [`packages/boot/app-boot/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/boot/app-boot/src/index.ts) | package entry point | The source note names this file directly. Core file in the package named by the note: `packages/boot/app-boot`. | `named-file, named-package-member, symbol-definition` |
| [`scripts/build-exe-for-python-sdk.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/build-exe-for-python-sdk.ts) | repository automation | The source note names this file directly. Contains the exact code literal `dsh-jsonrpc-agent-pkg` named by the note. | `exact-code-occurrence, named-file` |
| [`packages/examples/jsonrpc-demo/README.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/examples/jsonrpc-demo/README.md) | package contract and examples | Entry point or contract under the directory named by the note: `packages/examples/jsonrpc-demo`. The source note names this file directly. | `exact-code-occurrence, named-directory-member, named-file, named-package-member` |
| [`.github/workflows/build-exe-for-python-sdk.yml`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.github/workflows/build-exe-for-python-sdk.yml) | repository automation | The source note names this file directly. Contains the exact code literal `python/sdk` named by the note. | `exact-code-occurrence, named-file` |
| [`vendor/cordis/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/cordis/src/index.ts) | package entry point | Core file in the package named by the note: `vendor/cordis`. | `named-package-member` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `shutdown` | `const` | [`apps/cli/src/profile-boot.ts:210`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/cli/src/profile-boot.ts#L210) | `const shutdown = createProcessShutdown(async () => { await app.current?.fiber.dispose() })` |
| `loadEnv` | `function` | [`packages/boot/app-boot/src/index.ts:78`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/boot/app-boot/src/index.ts#L78) | `export function loadEnv(` |
| `installFailLoud` | `function` | [`packages/boot/app-boot/src/index.ts:609`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/boot/app-boot/src/index.ts#L609) | `export function installFailLoud(` |
| `boot` | `function` | [`packages/boot/app-boot/src/index.ts:757`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/boot/app-boot/src/index.ts#L757) | `export async function boot(` |
| `json` | `const` | [`packages/client/modules/src/index.ts:169`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/modules/src/index.ts#L169) | `const json = JSON.stringify(graph).replaceAll('<', '\\u003c')` |
| `node` | `const` | [`packages/core/tools/src/schema.ts:336`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/schema.ts#L336) | `const node: JsonSchemaNode = {}` |
| `baseUrl` | `const` | [`packages/llm/llm-pi-ai/src/catalog.ts:502`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm-pi-ai/src/catalog.ts#L502) | `const baseUrl = request.baseURL ?? base?.baseUrl ?? providerBaseUrl` |
| `exit` | `const` | [`packages/sdk/server/src/index.ts:57`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/sdk/server/src/index.ts#L57) | `const exit = config.exit ?? ((code: number): void => { process.exit(code) })` |
| `HarnessSdkJsonRpcServer` | `class` | [`packages/sdk/server/src/server.ts:53`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/sdk/server/src/server.ts#L53) | `export class HarnessSdkJsonRpcServer {` |
| `files` | `const` | [`packages/typert/generator/src/analyzer.ts:220`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/typert/generator/src/analyzer.ts#L220) | `const files = new Map<string, ts.SourceFile \| undefined>()` |
| `resolve_bundled_launch_args` | `def` | [`python/sdk-runtime/src/deepseek_harness_runtime/__init__.py:96`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/python/sdk-runtime/src/deepseek_harness_runtime/__init__.py#L96) | `def resolve_bundled_launch_args(mode: str \| None = None) -> tuple[str, ...]:` |
| `bin` | `const` | [`scripts/publish-npm-baseline.ts:457`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/publish-npm-baseline.ts#L457) | `const bin = resolve(consumerRoot, 'node_modules/@deepseek-ai/dsh/lib/bin.js')` |
| `url` | `const` | [`vendor/hmr/src/index.ts:257`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/hmr/src/index.ts#L257) | `const url = pathToFileURL(filename).href` |

### Tests and executable evidence

- [`apps/cli/tests/process-shutdown.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/cli/tests/process-shutdown.spec.ts) — A test under the owning area exercises or imports `shutdown`.
- [`apps/cli/tests/headless-shutdown.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/cli/tests/headless-shutdown.e2e.ts) — A test under the owning area exercises or imports `shutdown`.
- [`packages/sdk/server/tests/server.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/sdk/server/tests/server.spec.ts) — A test under the owning area exercises or imports `HarnessSdkJsonRpcServer`. A test under the owning area exercises or imports `deepseek-harness-sdk-runtime`.
- [`apps/cli/tests/fixtures/never-dispose.mjs`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/cli/tests/fixtures/never-dispose.mjs) — A test under the owning area exercises or imports `shutdown`.
- [`packages/boot/app-boot/tests/profile.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/boot/app-boot/tests/profile.spec.ts) — A test under the owning area exercises or imports `dsh-app-boot`.
- [`packages/boot/app-boot/tests/app-boot.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/boot/app-boot/tests/app-boot.spec.ts) — A test under the owning area exercises or imports `installFailLoud`. A test under the owning area exercises or imports `loadEnv`.
- [`packages/sdk/server/tests/plugin-apply.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/sdk/server/tests/plugin-apply.spec.ts) — A test under the owning area exercises or imports `dsh-sdk-jsonrpc-server`. A test under the owning area exercises or imports `deepseek-harness-sdk-runtime`.
- [`packages/sdk/server/tests/plugin-shape.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/sdk/server/tests/plugin-shape.spec.ts) — A test under the owning area exercises or imports `dsh-sdk-jsonrpc-server`.
- Source verification intent: The verification surface has three tiers. Mechanism tier: the measured conclusions for the --sea chain are embedded in the Decision sections (ESM dynamic import inside the VFS, single cordis instance, fail-loud config chain, node:sqlite, macOS ad-hoc signing runs). SDK tier: the complete keyless pytest suite covers the client protocol against a fake runtime peer, subprocess cleanup, absolute cwd propagation, dual-carrier launch, and carrier resolution; root CI runs it on Python 3.10.

## How to read the implementation

1. Start with [`.gitlab-ci.yml`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.gitlab-ci.yml) because it has the strongest evidence link to the note.
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
- **`rust/tokio-cancellation`** — Thread one cancellation token through async work, close owned resources before reuse, and test cancellation at every await boundary.
- **`rust/trait-seam`** — Use a small trait at the capability boundary; keep policy and model-facing schemas outside the backend implementation.

### Smallest safe implementation slice

1. Express the semantic contract as Rust types and one owning boundary.
2. Add the smallest adapter or service needed by the existing loop; do not add a new model-facing tool unless the four-tool surface cannot express the job.
3. Record durable evidence before deriving summaries or UI state.
4. Add one happy-path test, one interruption test, and one replay/restart test where state survives a turn.
5. Keep the patch reversible until the source note's strongest failure case is proven in the Rust harness.

## Search handles

- Tags: `class/architecture`, `concern/boundary`, `concern/compatibility`, `concern/discovery-routing`, `concern/evidence`, `concern/lifecycle`, `concern/ownership`, `concern/performance`, `concern/recovery`, `concern/schema-types`, `concern/simplification`, `domain/build-release`, `domain/configuration`, `domain/extensions`, `domain/filesystem`, `domain/llm`, `domain/multi-agent`, `domain/protocols`, `domain/session-state`, `domain/shell-terminal`, `domain/storage`, `domain/testing`, `lifecycle/implemented`, `mechanism/event-sourcing`, `mechanism/projection`, `mechanism/registry`, `mechanism/streaming`
- Aliases: `shutdown`, `loadEnv`, `installFailLoud`, `boot`, `json`, `node`, `baseUrl`, `exit`, `HarnessSdkJsonRpcServer`, `files`, `resolve_bundled_launch_args`, `bin`, `url`, `cordis.yml`
- Regex: `(?i)(shutdown|loadEnv|installFailLoud|boot|json|node|baseUrl|exit)`

```bash
rg -n --pcre2 "(?i)(shutdown|loadEnv|installFailLoud|boot|json|node|baseUrl|exit)" source-deepseek-harness
rg -l --fixed-strings "class/architecture" 03-agent-context
```

## Connected notes

- **`source-link`** — [0441. Python public publication workflow](0441-python-public-publication-workflow.md): The source note links to this decision directly.
- **`source-link`** — [0505. Required Python runtime pull-request validation](0505-required-python-runtime-pull-request-validation.md): The source note links to this decision directly.
- **`shares-code-with`** — [0075. Regroup packages/ by measured clustering](0075-regroup-packages-by-measured-clustering.md): Shares source implementation: `packages/boot/app-boot/src/index.ts`.
- **`same-design-pressure`** — [0146. The subprocess sandbox --- confinement seam, native runners, escalation, and per-session modes](0146-the-subprocess-sandbox-confinement-seam-native-runners-escalation-and-pe.md): Shares design concerns: `concern/boundary`, `concern/compatibility`, `concern/discovery-routing`.
- **`same-design-pressure`** — [0500. Keyless browser e2e lane for the web GUI](0500-keyless-browser-e2e-lane-for-the-web-gui.md): Shares design concerns: `concern/boundary`, `concern/compatibility`, `concern/discovery-routing`.
- **`same-design-pressure`** — [0509. Domain KV storage capability seam and the workspace entity](0509-domain-kv-storage-capability-seam-and-the-workspace-entity.md): Shares design concerns: `concern/boundary`, `concern/compatibility`, `concern/discovery-routing`.
- **`same-design-pressure`** — [0542. Dependency swaps rejected by the 2026-07 NIH audit](0542-dependency-swaps-rejected-by-the-2026-07-nih-audit.md): Shares design concerns: `concern/boundary`, `concern/compatibility`, `concern/discovery-routing`.
- **`same-design-pressure`** — [0557. Agent Client Protocol (ACP) support --- drive the coding agent from external editors](0557-agent-client-protocol-acp-support-drive-the-coding-agent-from-external-e.md): Shares design concerns: `concern/boundary`, `concern/compatibility`, `concern/discovery-routing`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0034-single-file-executable-sdk-runtime-distribution-single-exe.md`.
