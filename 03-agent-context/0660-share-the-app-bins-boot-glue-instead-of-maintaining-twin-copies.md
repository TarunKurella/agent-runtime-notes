---
id: "dsh-note-0660"
title: "Share the app bins' boot glue instead of maintaining twin copies"
status: "archived"
class: "simplification"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/archived/simplification/2026-07-04-share-app-bin-boot-glue.md"
implementation_evidence: "medium"
target_anchor: "append-only event and session store"
tags:
  - "class/simplification"
  - "concern/boundary"
  - "concern/evidence"
  - "concern/lifecycle"
  - "concern/ownership"
  - "concern/recovery"
  - "concern/schema-types"
  - "domain/build-release"
  - "domain/configuration"
  - "domain/extensions"
  - "domain/observability"
  - "domain/protocols"
  - "domain/session-state"
  - "domain/storage"
  - "lifecycle/archived"
  - "mechanism/event-sourcing"
  - "mechanism/policy"
aliases:
  - "resolveConfigPath"
  - "loadEnv"
  - "installFailLoud"
  - "assertEntriesLoaded"
  - "boot"
  - "@deepseek-ai/dsh-app-boot"
  - "packages/ui/app-boot"
  - "support/"
  - "bin.ts"
  - "ui/app-boot"
  - "start.ts"
  - "dsh-app-boot"
  - "Share the app bins' boot glue instead of maintaining twin copies"
  - "simplification"
search_regex: "(?i)(resolveConfigPath|loadEnv|installFailLoud|assertEntriesLoaded|boot|@deepseek\\-ai/dsh\\-app\\-boot|packages/ui/app\\-boot|support/)"
---

# 0660. Share the app bins' boot glue instead of maintaining twin copies — implementation context

## Open this when

The stdio and ACP bins duplicated environment loading, fail-loud handling, entry validation, and boot logic, including subtle Loader failure behavior. Their copies had already drifted and lived in self-executing files excluded from unit coverage, making their helper exports unusable.

## Source decision

The helpers live once, in @deepseek-ai/dsh-app-boot (packages/ui/app-boot, in the ui group because the bins are published artifacts whose runtime dependency must itself be published, not support/): resolveConfigPath (snapshot-aware, the single path resolver for both bins), loadEnv, installFailLoud, assertEntriesLoaded, and boot, each parameterized by the bin's diagnostic prefix and injectable at its side-effect seams (the warn sink, the process slice) so the unit suite covers every branch --- including boot() driven in-process against the real Loader with relative-specifier configs, both the settled-tree happy.

## Decision status

Historical design. Use it to understand an older pressure or failure; locate the replacing decision before implementing anything.

- Raw note: [archived/simplification/2026-07-04-share-app-bin-boot-glue.md](../02-notes/archived/simplification/2026-07-04-share-app-bin-boot-glue.md)
- Pinned source: [.agents/notes/archived/simplification/2026-07-04-share-app-bin-boot-glue.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/archived/simplification/2026-07-04-share-app-bin-boot-glue.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`packages/boot/app-boot/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/boot/app-boot/src/index.ts) | package entry point | Core file in the package named by the note: `packages/boot/app-boot`. Defines `resolveConfigPath`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/boot/app-boot/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/boot/app-boot/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/boot/app-boot`. | `named-package-member` |
| [`packages/boot/app-boot`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/packages/boot/app-boot) | package or module directory | The note names this package or capability. | `named-package` |
| [`packages/boot/app-boot/README.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/boot/app-boot/README.md) | package contract and examples | Core file in the package named by the note: `packages/boot/app-boot`. | `named-package-member` |
| [`packages/boot/app-boot/package.json`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/boot/app-boot/package.json) | composition and configuration | Core file in the package named by the note: `packages/boot/app-boot`. | `named-package-member` |
| [`pnpm-lock.yaml`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/pnpm-lock.yaml) | composition and configuration | Contains the exact code literal `dsh-app-boot` named by the note. | `exact-code-occurrence` |
| [`examples/package.json`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/examples/package.json) | composition and configuration | Contains the exact code literal `dsh-app-boot` named by the note. | `exact-code-occurrence` |
| [`apps/cli/package.json`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/cli/package.json) | composition and configuration | Contains the exact code literal `dsh-app-boot` named by the note. | `exact-code-occurrence` |
| [`docs/config-catalog.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/config-catalog.md) | package contract and examples | Contains the exact code literal `dsh-app-boot` named by the note. | `exact-code-occurrence` |
| [`docs/config-catalog.zh.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/config-catalog.zh.md) | package contract and examples | Contains the exact code literal `dsh-app-boot` named by the note. | `exact-code-occurrence` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `resolveConfigPath` | `function` | [`packages/boot/app-boot/src/index.ts:61`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/boot/app-boot/src/index.ts#L61) | `export function resolveConfigPath(` |
| `loadEnv` | `function` | [`packages/boot/app-boot/src/index.ts:78`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/boot/app-boot/src/index.ts#L78) | `export function loadEnv(` |
| `installFailLoud` | `function` | [`packages/boot/app-boot/src/index.ts:609`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/boot/app-boot/src/index.ts#L609) | `export function installFailLoud(` |
| `assertEntriesLoaded` | `function` | [`packages/boot/app-boot/src/index.ts:658`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/boot/app-boot/src/index.ts#L658) | `export function assertEntriesLoaded(ctx: Context, binName: string): void {` |
| `boot` | `function` | [`packages/boot/app-boot/src/index.ts:757`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/boot/app-boot/src/index.ts#L757) | `export async function boot(` |

### Tests and executable evidence

- [`packages/boot/app-boot/tests/profile.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/boot/app-boot/tests/profile.spec.ts) — A test under the owning area exercises or imports `dsh-app-boot`.
- [`packages/boot/app-boot/tests/app-boot.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/boot/app-boot/tests/app-boot.spec.ts) — A test under the owning area exercises or imports `resolveConfigPath`. A test under the owning area exercises or imports `loadEnv`.
- [`packages/boot/app-boot/tests/user-patches.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/boot/app-boot/tests/user-patches.spec.ts) — A test under the owning area exercises or imports `dsh-app-boot`.
- [`apps/web/tests/scaffold.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/scaffold.ts) — Contains the exact code literal `dsh-app-boot` named by the note.
- [`apps/cli/tests/windows-shell.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/cli/tests/windows-shell.spec.ts) — Contains the exact code literal `dsh-app-boot` named by the note.
- [`apps/cli/tests/web-agent-presets.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/cli/tests/web-agent-presets.e2e.ts) — Contains the exact code literal `dsh-app-boot` named by the note.

## How to read the implementation

1. Start with [`packages/boot/app-boot/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/boot/app-boot/src/index.ts) because it has the strongest evidence link to the note.
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
- **Status gate** — Historical design. Use it to understand an older pressure or failure; locate the replacing decision before implementing anything.

### Smallest safe implementation slice

1. Express the semantic contract as Rust types and one owning boundary.
2. Add the smallest adapter or service needed by the existing loop; do not add a new model-facing tool unless the four-tool surface cannot express the job.
3. Record durable evidence before deriving summaries or UI state.
4. Add one happy-path test, one interruption test, and one replay/restart test where state survives a turn.
5. Keep the patch reversible until the source note's strongest failure case is proven in the Rust harness.

## Search handles

- Tags: `class/simplification`, `concern/boundary`, `concern/evidence`, `concern/lifecycle`, `concern/ownership`, `concern/recovery`, `concern/schema-types`, `domain/build-release`, `domain/configuration`, `domain/extensions`, `domain/observability`, `domain/protocols`, `domain/session-state`, `domain/storage`, `lifecycle/archived`, `mechanism/event-sourcing`, `mechanism/policy`
- Aliases: `resolveConfigPath`, `loadEnv`, `installFailLoud`, `assertEntriesLoaded`, `boot`, `@deepseek-ai/dsh-app-boot`, `packages/ui/app-boot`, `support/`, `bin.ts`, `ui/app-boot`, `start.ts`, `dsh-app-boot`, `Share the app bins' boot glue instead of maintaining twin copies`, `simplification`
- Regex: `(?i)(resolveConfigPath|loadEnv|installFailLoud|assertEntriesLoaded|boot|@deepseek\-ai/dsh\-app\-boot|packages/ui/app\-boot|support/)`

```bash
rg -n --pcre2 "(?i)(resolveConfigPath|loadEnv|installFailLoud|assertEntriesLoaded|boot|@deepseek\\-ai/dsh\\-app\\-boot|packages/ui/app\\-boot|support/)" source-deepseek-harness
rg -l --fixed-strings "class/simplification" 03-agent-context
```

## Connected notes

- **`source-link`** — [0546. Extract example apps into packages](0546-extract-example-apps-into-packages.md): The source note links to this decision directly.
- **`shares-code-with`** — [0325. Source checkout paths do not define working directories](0325-source-checkout-paths-do-not-define-working-directories.md): Shares source implementation: `packages/boot/app-boot`, `packages/boot/app-boot/README.md`.
- **`shares-code-with`** — [0602. dsh --dump-config prints the composed config tree](0602-dsh-dump-config-prints-the-composed-config-tree.md): Shares source implementation: `packages/boot/app-boot`, `packages/boot/app-boot/README.md`.
- **`shares-code-with`** — [0096. Splitting the credential store from the user environment layer](0096-splitting-the-credential-store-from-the-user-environment-layer.md): Shares source implementation: `packages/boot/app-boot`, `packages/boot/app-boot/README.md`.
- **`shares-code-with`** — [0556. Native TypeScript source launch for dsh](0556-native-typescript-source-launch-for-dsh.md): Shares source implementation: `packages/boot/app-boot`, `packages/boot/app-boot/src/index.ts`.
- **`shares-code-with`** — [0166. The dsh CLI and personal config overlays from the Harness home](0166-the-dsh-cli-and-personal-config-overlays-from-the-harness-home.md): Shares source implementation: `packages/boot/app-boot/src/index.ts`, `packages/boot/app-boot/src/invariant.ts`.
- **`shares-code-with`** — [0145. Explicit model-facing tool order](0145-explicit-model-facing-tool-order.md): Shares source implementation: `packages/boot/app-boot/src/index.ts`, `packages/boot/app-boot/src/invariant.ts`.
- **`shares-code-with`** — [0553. Parse `dsh` argv through one Commander adapter](0553-parse-dsh-argv-through-one-commander-adapter.md): Shares source implementation: `packages/boot/app-boot/src/index.ts`, `packages/boot/app-boot/src/invariant.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0660-share-the-app-bins-boot-glue-instead-of-maintaining-twin-copies.md`.
