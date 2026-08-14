---
id: "dsh-note-0602"
title: "dsh --dump-config prints the composed config tree"
status: "archived"
class: "feature"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/archived/feature/2026-07-30-dsh-dump-config.md"
implementation_evidence: "high"
target_anchor: "append-only event and session store"
tags:
  - "class/feature"
  - "concern/boundary"
  - "concern/discovery-routing"
  - "concern/evidence"
  - "concern/lifecycle"
  - "concern/ownership"
  - "concern/recovery"
  - "domain/build-release"
  - "domain/configuration"
  - "domain/extensions"
  - "domain/session-state"
  - "domain/storage"
  - "domain/ui-interaction"
  - "lifecycle/archived"
  - "mechanism/event-sourcing"
  - "mechanism/projection"
aliases:
  - "renderConfigDump"
  - "insert"
  - "entryListSchema"
  - "applyEntryPatches"
  - "config"
  - "--config"
  - "~/.dsh/config.yaml"
  - "dsh --dump-config"
  - "dsh web --dump-config"
  - "dsh --dump-default-config"
  - "dsh web --dump-default-config"
  - "applyPatches"
  - "dsh-app-boot"
  - "apps/cli/src/dump-config.ts"
search_regex: "(?i)(renderConfigDump|insert|entryListSchema|applyEntryPatches|config|\\-\\-config|\\~/\\.dsh/config\\.yaml|dsh[- ]\\-\\-dump\\-config)"
---

# 0602. dsh --dump-config prints the composed config tree — implementation context

## Open this when

The booted tree is a composition the user never sees: the shipped base, a surface overlay, and the --config or personal ~/.dsh/config.yaml overlay apply as sibling patch lists where each id-targeted patch replaces the row's whole config and an unmatched id only warns. Debugging a misbehaving personal overlay (a restated field dropped, a row id typo, a patch applying to the wrong surface) required mentally replaying the patch algorithm across three files. There was no way to see the effective tree or to diff it against the shipped defaults.

## Source decision

dsh --dump-config and dsh web --dump-config print the composed entry list --- base, surface overlay, then the --config or personal overlay, exactly the layers that surface's boot assembles --- as YAML on stdout and exit without booting. dsh --dump-default-config / dsh web --dump-default-config stop at the surface overlay, so diffing the two outputs shows precisely what the user layer changes.

## Decision status

Historical design. Use it to understand an older pressure or failure; locate the replacing decision before implementing anything.

- Raw note: [archived/feature/2026-07-30-dsh-dump-config.md](../02-notes/archived/feature/2026-07-30-dsh-dump-config.md)
- Pinned source: [.agents/notes/archived/feature/2026-07-30-dsh-dump-config.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/archived/feature/2026-07-30-dsh-dump-config.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`apps/cli/src/dump-config.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/cli/src/dump-config.ts) | runtime implementation | The source note names this file directly. | `named-file` |
| [`packages/boot/app-boot/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/boot/app-boot/src/index.ts) | package entry point | Core file in the package named by the note: `packages/boot/app-boot`. Defines `renderConfigDump`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/boot/app-boot/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/boot/app-boot/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/boot/app-boot`. | `named-package-member` |
| [`packages/boot/app-boot`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/packages/boot/app-boot) | package or module directory | The note names this package or capability. | `named-package` |
| [`vendor/loader/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/loader/src/index.ts) | package entry point | Defines `config`, a construct named by the note. | `symbol-definition` |
| [`vendor/include/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/include/src/index.ts) | package entry point | Defines `applyEntryPatches`, a construct named by the note. Defines `entryListSchema`, a construct named by the note. | `symbol-definition` |
| [`packages/session-query/session-query-sqlite/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/session-query/session-query-sqlite/src/index.ts) | package entry point | Defines `insert`, a construct named by the note. | `symbol-definition` |
| [`packages/boot/app-boot/README.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/boot/app-boot/README.md) | package contract and examples | Core file in the package named by the note: `packages/boot/app-boot`. | `named-package-member` |
| [`packages/boot/app-boot/package.json`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/boot/app-boot/package.json) | composition and configuration | Core file in the package named by the note: `packages/boot/app-boot`. | `named-package-member` |
| [`pnpm-lock.yaml`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/pnpm-lock.yaml) | composition and configuration | Contains the exact code literal `dsh-app-boot` named by the note. | `exact-code-occurrence` |
| [`docs/testing.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/testing.md) | package contract and examples | Contains the exact code literal `lib/bin.js` named by the note. | `exact-code-occurrence` |
| [`docs/testing.zh.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/testing.zh.md) | package contract and examples | Contains the exact code literal `lib/bin.js` named by the note. | `exact-code-occurrence` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `renderConfigDump` | `function` | [`packages/boot/app-boot/src/index.ts:379`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/boot/app-boot/src/index.ts#L379) | `export function renderConfigDump(` |
| `insert` | `const` | [`packages/session-query/session-query-sqlite/src/index.ts:584`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/session-query/session-query-sqlite/src/index.ts#L584) | `const insert = db.prepare(\`` |
| `entryListSchema` | `const` | [`vendor/include/src/index.ts:23`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/include/src/index.ts#L23) | `export const entryListSchema = yaml.JSON_SCHEMA.extend(JsExpr)` |
| `applyEntryPatches` | `function` | [`vendor/include/src/index.ts:58`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/include/src/index.ts#L58) | `export function applyEntryPatches(` |
| `config` | `const` | [`vendor/loader/src/index.ts:93`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/loader/src/index.ts#L93) | `const config = next()` |

### Tests and executable evidence

- [`packages/boot/app-boot/tests/profile.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/boot/app-boot/tests/profile.spec.ts) — A test under the owning area exercises or imports `dsh-app-boot`.
- [`packages/boot/app-boot/tests/config-dump.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/boot/app-boot/tests/config-dump.spec.ts) — A test under the owning area exercises or imports `applyEntryPatches`. A test under the owning area exercises or imports `entryListSchema`.
- [`packages/boot/app-boot/tests/user-patches.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/boot/app-boot/tests/user-patches.spec.ts) — A test under the owning area exercises or imports `dsh-app-boot`.
- [`apps/web/tests/scaffold.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/scaffold.ts) — Contains the exact code literal `dsh-app-boot` named by the note.
- [`apps/cli/tests/built-bin.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/cli/tests/built-bin.e2e.ts) — Contains the exact code literal `lib/bin.js` named by the note.
- [`scripts/release/families.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/release/families.spec.ts) — Contains the exact code literal `lib/bin.js` named by the note.
- [`apps/cli/tests/windows-shell.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/cli/tests/windows-shell.spec.ts) — Contains the exact code literal `dsh-app-boot` named by the note.
- [`apps/cli/tests/web-agent-presets.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/cli/tests/web-agent-presets.e2e.ts) — Contains the exact code literal `dsh-app-boot` named by the note.

## How to read the implementation

1. Start with [`apps/cli/src/dump-config.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/cli/src/dump-config.ts) because it has the strongest evidence link to the note.
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
- **`rust/process-discipline`** — Treat this as repository or release policy unless it changes a runtime contract. Do not add runtime machinery for a process-only lesson.
- **`rust/quickjs-boundary`** — QuickJS holds orchestration variables; Rust/Tokio owns async execution and exposes only explicitly granted capabilities.
- **`rust/trait-seam`** — Use a small trait at the capability boundary; keep policy and model-facing schemas outside the backend implementation.
- **Status gate** — Historical design. Use it to understand an older pressure or failure; locate the replacing decision before implementing anything.

### Smallest safe implementation slice

1. Express the semantic contract as Rust types and one owning boundary.
2. Add the smallest adapter or service needed by the existing loop; do not add a new model-facing tool unless the four-tool surface cannot express the job.
3. Record durable evidence before deriving summaries or UI state.
4. Add one happy-path test, one interruption test, and one replay/restart test where state survives a turn.
5. Keep the patch reversible until the source note's strongest failure case is proven in the Rust harness.

## Search handles

- Tags: `class/feature`, `concern/boundary`, `concern/discovery-routing`, `concern/evidence`, `concern/lifecycle`, `concern/ownership`, `concern/recovery`, `domain/build-release`, `domain/configuration`, `domain/extensions`, `domain/session-state`, `domain/storage`, `domain/ui-interaction`, `lifecycle/archived`, `mechanism/event-sourcing`, `mechanism/projection`
- Aliases: `renderConfigDump`, `insert`, `entryListSchema`, `applyEntryPatches`, `config`, `--config`, `~/.dsh/config.yaml`, `dsh --dump-config`, `dsh web --dump-config`, `dsh --dump-default-config`, `dsh web --dump-default-config`, `applyPatches`, `dsh-app-boot`, `apps/cli/src/dump-config.ts`
- Regex: `(?i)(renderConfigDump|insert|entryListSchema|applyEntryPatches|config|\-\-config|\~/\.dsh/config\.yaml|dsh[- ]\-\-dump\-config)`

```bash
rg -n --pcre2 "(?i)(renderConfigDump|insert|entryListSchema|applyEntryPatches|config|\\-\\-config|\\~/\\.dsh/config\\.yaml|dsh[- ]\\-\\-dump\\-config)" source-deepseek-harness
rg -l --fixed-strings "class/feature" 03-agent-context
```

## Connected notes

- **`shares-code-with`** — [0660. Share the app bins' boot glue instead of maintaining twin copies](0660-share-the-app-bins-boot-glue-instead-of-maintaining-twin-copies.md): Shares source implementation: `packages/boot/app-boot`, `packages/boot/app-boot/README.md`.
- **`shares-code-with`** — [0325. Source checkout paths do not define working directories](0325-source-checkout-paths-do-not-define-working-directories.md): Shares source implementation: `packages/boot/app-boot`, `packages/boot/app-boot/README.md`.
- **`shares-code-with`** — [0096. Splitting the credential store from the user environment layer](0096-splitting-the-credential-store-from-the-user-environment-layer.md): Shares source implementation: `packages/boot/app-boot`, `packages/boot/app-boot/README.md`.
- **`shares-code-with`** — [0556. Native TypeScript source launch for dsh](0556-native-typescript-source-launch-for-dsh.md): Shares source implementation: `packages/boot/app-boot`, `packages/boot/app-boot/src/index.ts`.
- **`shares-code-with`** — [0546. Extract example apps into packages](0546-extract-example-apps-into-packages.md): Shares source implementation: `packages/boot/app-boot/src/index.ts`, `packages/boot/app-boot/src/invariant.ts`.
- **`shares-code-with`** — [0166. The dsh CLI and personal config overlays from the Harness home](0166-the-dsh-cli-and-personal-config-overlays-from-the-harness-home.md): Shares source implementation: `packages/boot/app-boot/src/index.ts`, `packages/boot/app-boot/src/invariant.ts`.
- **`shares-code-with`** — [0553. Parse `dsh` argv through one Commander adapter](0553-parse-dsh-argv-through-one-commander-adapter.md): Shares source implementation: `packages/boot/app-boot/src/index.ts`, `packages/boot/app-boot/src/invariant.ts`.
- **`shares-code-with`** — [0145. Explicit model-facing tool order](0145-explicit-model-facing-tool-order.md): Shares source implementation: `packages/boot/app-boot/src/index.ts`, `packages/boot/app-boot/src/invariant.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0602-dsh-dump-config-prints-the-composed-config-tree.md`.
