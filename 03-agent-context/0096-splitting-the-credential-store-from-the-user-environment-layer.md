---
id: "dsh-note-0096"
title: "Splitting the credential store from the user environment layer"
status: "implemented"
class: "architecture"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/architecture/2026-08-04-credentials-yaml-and-user-environment-layer.md"
implementation_evidence: "medium"
target_anchor: "sandbox capability boundary"
tags:
  - "class/architecture"
  - "concern/boundary"
  - "concern/compatibility"
  - "concern/concurrency"
  - "concern/ownership"
  - "concern/recovery"
  - "concern/schema-types"
  - "concern/simplification"
  - "concern/trust"
  - "domain/build-release"
  - "domain/configuration"
  - "domain/llm"
  - "domain/observability"
  - "domain/protocols"
  - "domain/security"
  - "domain/shell-terminal"
  - "domain/storage"
  - "lifecycle/implemented"
aliases:
  - "loadEnv"
  - "loadLayeredEnv"
  - "CredentialRef"
  - "version"
  - "env"
  - "set"
  - "$DSH_HOME/.env"
  - "credentials-local"
  - "process.env"
  - "DEEPSEEK_BASE_URL"
  - "DEEPSEEK_API_KEY"
  - ".env"
  - ".credentials.yaml"
  - "dsh-app-boot"
search_regex: "(?i)(loadEnv|loadLayeredEnv|CredentialRef|version|\\$DSH_HOME/\\.env|credentials\\-local|process\\.env|DEEPSEEK_BASE_URL)"
---

# 0096. Splitting the credential store from the user environment layer — implementation context

## Open this when

$DSH_HOME/.env carried two incompatible jobs. It was the writable secret store of credentials-local, so no surface could hoist it into process.env --- hoisting would make every stored key read as a read-only launch override and block rotation from the Models page. But its name and dotenv format promise an environment file, so users put non-secrets in it and those values reached nothing: a DEEPSEEK_BASE_URL beside a working DEEPSEEK_API_KEY in the same file was silently ignored, because only the credential provider read the document and it addresses credential references alone.

## Source decision

The two jobs become two files under the Harness home. .credentials.yaml is the provider-managed store. A strict YAML mapping of CredentialRef to non-empty string, with no version field and no wrapper level: Because the document holds credentials and nothing else, every deviation is a rejection rather than a skipped entry: a non-mapping root, a key that is not a POSIX identifier, a non-string value, an empty string, a duplicate key, and malformed YAML all fail --- loud at boot and at a write, warn-and-keep-the-last-good-snapshot on a live reload.

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/architecture/2026-08-04-credentials-yaml-and-user-environment-layer.md](../02-notes/implemented/architecture/2026-08-04-credentials-yaml-and-user-environment-layer.md)
- Pinned source: [.agents/notes/implemented/architecture/2026-08-04-credentials-yaml-and-user-environment-layer.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/architecture/2026-08-04-credentials-yaml-and-user-environment-layer.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`packages/boot/app-boot/README.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/boot/app-boot/README.md) | package contract and examples | The source note names this file directly. Core file in the package named by the note: `packages/boot/app-boot`. | `named-file, named-package-member` |
| [`packages/subprocess/subprocess/README.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/subprocess/subprocess/README.md) | package contract and examples | The source note names this file directly. | `named-file` |
| [`packages/credentials/credentials-local/README.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/credentials/credentials-local/README.md) | package contract and examples | The source note names this file directly. Core file in the package named by the note: `packages/credentials/credentials-local`. | `named-file, named-package-member` |
| [`packages/boot/app-boot/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/boot/app-boot/src/index.ts) | package entry point | Core file in the package named by the note: `packages/boot/app-boot`. Defines `loadLayeredEnv`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/boot/app-boot/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/boot/app-boot/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/boot/app-boot`. | `named-package-member` |
| [`packages/credentials/credentials-local/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/credentials/credentials-local/src/index.ts) | package entry point | Core file in the package named by the note: `packages/credentials/credentials-local`. | `named-package-member` |
| [`packages/credentials/credentials-local/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/credentials/credentials-local/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/credentials/credentials-local`. | `named-package-member` |
| [`packages/boot/app-boot`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/packages/boot/app-boot) | package or module directory | The note names this package or capability. | `named-package` |
| [`packages/credentials/credentials-local`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/packages/credentials/credentials-local) | package or module directory | The note names this package or capability. | `named-package` |
| [`packages/e2b/fs-e2b/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/e2b/fs-e2b/src/index.ts) | package entry point | Defines `version`, a construct named by the note. | `symbol-definition` |
| [`packages/subprocess/subprocess/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/subprocess/subprocess/src/index.ts) | package entry point | Defines `env`, a construct named by the note. | `symbol-definition` |
| [`packages/credentials/credentials/src/types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/credentials/credentials/src/types.ts) | public types and contract | Defines `CredentialRef`, a construct named by the note. | `symbol-definition` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `loadEnv` | `function` | [`packages/boot/app-boot/src/index.ts:78`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/boot/app-boot/src/index.ts#L78) | `export function loadEnv(` |
| `loadLayeredEnv` | `function` | [`packages/boot/app-boot/src/index.ts:177`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/boot/app-boot/src/index.ts#L177) | `export function loadLayeredEnv(` |
| `CredentialRef` | `type` | [`packages/credentials/credentials/src/types.ts:13`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/credentials/credentials/src/types.ts#L13) | `export type CredentialRef = Branded<'CredentialRef'>` |
| `version` | `const` | [`packages/e2b/fs-e2b/src/index.ts:389`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/e2b/fs-e2b/src/index.ts#L389) | `const version = await this.writeAtomic(` |
| `env` | `const` | [`packages/subprocess/subprocess/src/index.ts:61`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/subprocess/subprocess/src/index.ts#L61) | `const env: Record<string, string> = {}` |
| `set` | `const` | [`packages/test-support/client-runtime/src/sessions.ts:52`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/test-support/client-runtime/src/sessions.ts#L52) | `const set = listeners.get(key) ?? new Set()` |

### Tests and executable evidence

- [`packages/boot/app-boot/tests/profile.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/boot/app-boot/tests/profile.spec.ts) — A test under the owning area exercises or imports `dsh-app-boot`.
- [`packages/boot/app-boot/tests/app-boot.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/boot/app-boot/tests/app-boot.spec.ts) — A test under the owning area exercises or imports `loadLayeredEnv`. A test under the owning area exercises or imports `loadEnv`.
- [`packages/credentials/credentials/tests/memory.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/credentials/credentials/tests/memory.ts) — A test under the owning area exercises or imports `CredentialRef`.
- [`packages/boot/app-boot/tests/user-patches.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/boot/app-boot/tests/user-patches.spec.ts) — A test under the owning area exercises or imports `dsh-app-boot`.
- [`packages/credentials/credentials/tests/credentials.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/credentials/credentials/tests/credentials.spec.ts) — A test under the owning area exercises or imports `CredentialRef`.
- [`packages/credentials/credentials-local/tests/local.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/credentials/credentials-local/tests/local.spec.ts) — A test under the owning area exercises or imports `CredentialRef`.
- [`apps/web/tests/scaffold.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/scaffold.ts) — Contains the exact code literal `dsh-app-boot` named by the note.
- [`apps/cli/tests/windows-shell.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/cli/tests/windows-shell.spec.ts) — Contains the exact code literal `dsh-app-boot` named by the note.

## How to read the implementation

1. Start with [`packages/boot/app-boot/README.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/boot/app-boot/README.md) because it has the strongest evidence link to the note.
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

- Tags: `class/architecture`, `concern/boundary`, `concern/compatibility`, `concern/concurrency`, `concern/ownership`, `concern/recovery`, `concern/schema-types`, `concern/simplification`, `concern/trust`, `domain/build-release`, `domain/configuration`, `domain/llm`, `domain/observability`, `domain/protocols`, `domain/security`, `domain/shell-terminal`, `domain/storage`, `lifecycle/implemented`
- Aliases: `loadEnv`, `loadLayeredEnv`, `CredentialRef`, `version`, `env`, `set`, `$DSH_HOME/.env`, `credentials-local`, `process.env`, `DEEPSEEK_BASE_URL`, `DEEPSEEK_API_KEY`, `.env`, `.credentials.yaml`, `dsh-app-boot`
- Regex: `(?i)(loadEnv|loadLayeredEnv|CredentialRef|version|\$DSH_HOME/\.env|credentials\-local|process\.env|DEEPSEEK_BASE_URL)`

```bash
rg -n --pcre2 "(?i)(loadEnv|loadLayeredEnv|CredentialRef|version|\\$DSH_HOME/\\.env|credentials\\-local|process\\.env|DEEPSEEK_BASE_URL)" source-deepseek-harness
rg -l --fixed-strings "class/architecture" 03-agent-context
```

## Connected notes

- **`source-link`** — [0077. request-level LLM configuration and the credential seam](0077-request-level-llm-configuration-and-the-credential-seam.md): The source note links to this decision directly.
- **`shares-code-with`** — [0660. Share the app bins' boot glue instead of maintaining twin copies](0660-share-the-app-bins-boot-glue-instead-of-maintaining-twin-copies.md): Shares source implementation: `packages/boot/app-boot`, `packages/boot/app-boot/README.md`.
- **`shares-code-with`** — [0086. settings write-path integrity and observer lifecycle](0086-settings-write-path-integrity-and-observer-lifecycle.md): Shares source implementation: `packages/credentials/credentials-local`, `packages/credentials/credentials-local/src/index.ts`.
- **`shares-code-with`** — [0325. Source checkout paths do not define working directories](0325-source-checkout-paths-do-not-define-working-directories.md): Shares source implementation: `packages/boot/app-boot`, `packages/boot/app-boot/README.md`.
- **`shares-code-with`** — [0602. dsh --dump-config prints the composed config tree](0602-dsh-dump-config-prints-the-composed-config-tree.md): Shares source implementation: `packages/boot/app-boot`, `packages/boot/app-boot/README.md`.
- **`shares-code-with`** — [0083. credential boundaries, whole-snapshot requests, and atomic route registration](0083-credential-boundaries-whole-snapshot-requests-and-atomic-route-registrat.md): Shares source implementation: `packages/credentials/credentials-local`, `packages/credentials/credentials-local/src/index.ts`.
- **`shares-code-with`** — [0556. Native TypeScript source launch for dsh](0556-native-typescript-source-launch-for-dsh.md): Shares source implementation: `packages/boot/app-boot`, `packages/boot/app-boot/src/index.ts`.
- **`shares-code-with`** — [0553. Parse `dsh` argv through one Commander adapter](0553-parse-dsh-argv-through-one-commander-adapter.md): Shares source implementation: `packages/boot/app-boot/src/index.ts`, `packages/boot/app-boot/src/invariant.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0096-splitting-the-credential-store-from-the-user-environment-layer.md`.
