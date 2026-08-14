---
id: "dsh-note-0086"
title: "settings write-path integrity and observer lifecycle"
status: "implemented"
class: "architecture"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/architecture/2026-07-30-settings-write-path-integrity.md"
implementation_evidence: "medium"
target_anchor: "sandbox capability boundary"
tags:
  - "class/architecture"
  - "concern/boundary"
  - "concern/cancellation-timeout"
  - "concern/compatibility"
  - "concern/concurrency"
  - "concern/evidence"
  - "concern/human-control"
  - "concern/lifecycle"
  - "concern/ownership"
  - "concern/performance"
  - "concern/recovery"
  - "concern/schema-types"
  - "concern/simplification"
  - "domain/build-release"
  - "domain/configuration"
  - "domain/extensions"
  - "domain/llm"
  - "domain/observability"
  - "domain/protocols"
  - "domain/security"
  - "domain/storage"
  - "lifecycle/implemented"
  - "mechanism/generation"
  - "mechanism/policy"
  - "mechanism/registry"
aliases:
  - "watch"
  - "active"
  - "deepEqualJson"
  - "cloneJsonShaped"
  - "ready"
  - "dsh-settings-file"
  - "dsh-settings"
  - "settings/updated"
  - "structuredClone"
  - "persistSection"
  - "reconcileFromDisk"
  - "<file>.lock"
  - "pendingTails"
  - "INVARIANT"
search_regex: "(?i)(watch|active|deepEqualJson|cloneJsonShaped|ready|dsh\\-settings\\-file|dsh\\-settings|settings/updated)"
---

# 0086. settings write-path integrity and observer lifecycle — implementation context

## Open this when

The provider's write path could destroy state it never observed, and the Service Definition's observer lifecycle leaked past disposal. Concretely: watcher reloads and document writes ran on two independent promise chains while every write rendered the whole next document from the cached text, so an external edit still inside the debounce window was overwritten --- and the follow-up reload no-oped because the post-rename content matched the cache, erasing the edit without a trace. The initial load() raced the watcher's own setup, leaving a startup window whose changes never fire an event.

## Source decision

One operation chain, and every write is a read-modify-write. Watcher refreshes and persists from every namespace queue share a single settled chain, and persistSection begins by reconciling the on-disk text into the seam --- publishing any unobserved difference first --- before rendering against that fresh text. A write can no longer resurrect a stale document, and an on-disk document that turned invalid fails the write loud rather than being overwritten (the reload path keeps its warn-and-keep-last-good policy; the shared reconcileFromDisk throws and each caller picks its policy).

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/architecture/2026-07-30-settings-write-path-integrity.md](../02-notes/implemented/architecture/2026-07-30-settings-write-path-integrity.md)
- Pinned source: [.agents/notes/implemented/architecture/2026-07-30-settings-write-path-integrity.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/architecture/2026-07-30-settings-write-path-integrity.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`packages/settings/settings/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/settings/settings/src/index.ts) | package entry point | Core file in the package named by the note: `packages/settings/settings`. Defines `cloneJsonShaped`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/settings/settings/src/types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/settings/settings/src/types.ts) | public types and contract | Core file in the package named by the note: `packages/settings/settings`. | `named-package-member` |
| [`packages/settings/settings/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/settings/settings/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/settings/settings`. | `named-package-member` |
| [`packages/settings/settings-file/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/settings/settings-file/src/index.ts) | package entry point | Core file in the package named by the note: `packages/settings/settings-file`. | `named-package-member` |
| [`packages/settings/settings-file/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/settings/settings-file/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/settings/settings-file`. | `named-package-member` |
| [`packages/credentials/credentials-local/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/credentials/credentials-local/src/index.ts) | package entry point | Core file in the package named by the note: `packages/credentials/credentials-local`. | `named-package-member` |
| [`packages/credentials/credentials-local/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/credentials/credentials-local/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/credentials/credentials-local`. | `named-package-member` |
| [`packages/settings/settings`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/packages/settings/settings) | package or module directory | The note names this package or capability. | `named-package` |
| [`packages/settings/settings-file`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/packages/settings/settings-file) | package or module directory | The note names this package or capability. | `named-package` |
| [`packages/credentials/credentials-local`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/packages/credentials/credentials-local) | package or module directory | The note names this package or capability. | `named-package` |
| [`vendor/hmr/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/hmr/src/index.ts) | package entry point | Defines `ready`, a construct named by the note. | `symbol-definition` |
| [`packages/client/hmr/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/hmr/src/index.ts) | package entry point | Defines `watch`, a construct named by the note. | `symbol-definition` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `watch` | `const` | [`packages/client/hmr/src/index.ts:91`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/hmr/src/index.ts#L91) | `const watch = { path, mtimeMs: baseline.mtimeMs, size: baseline.size, dirty: false }` |
| `active` | `let` | [`packages/core/scope/src/store.ts:47`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/scope/src/store.ts#L47) | `let active = true` |
| `deepEqualJson` | `function` | [`packages/settings/settings/src/index.ts:145`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/settings/settings/src/index.ts#L145) | `export function deepEqualJson(a: unknown, b: unknown): boolean {` |
| `cloneJsonShaped` | `function` | [`packages/settings/settings/src/index.ts:253`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/settings/settings/src/index.ts#L253) | `function cloneJsonShaped(` |
| `ready` | `const` | [`vendor/hmr/src/index.ts:160`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/hmr/src/index.ts#L160) | `const ready = Promise.withResolvers<void>()` |

### Tests and executable evidence

- [`packages/settings/settings/tests/settings.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/settings/settings/tests/settings.spec.ts) — A test under the owning area exercises or imports `INVARIANT`. A test under the owning area exercises or imports `deepEqualJson`.
- [`packages/settings/settings-file/tests/watcher.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/settings/settings-file/tests/watcher.spec.ts) — A test under the owning area exercises or imports `INVARIANT`.
- [`packages/credentials/credentials-local/tests/watcher.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/credentials/credentials-local/tests/watcher.spec.ts) — A test under the owning area exercises or imports `INVARIANT`.
- [`packages/settings/settings-file/tests/loader-composition.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/settings/settings-file/tests/loader-composition.spec.ts) — A test under the owning area exercises or imports `dsh-settings-file`.
- [`packages/credentials/credentials-local/tests/review-fixes.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/credentials/credentials-local/tests/review-fixes.spec.ts) — A test under the owning area exercises or imports `INVARIANT`.
- [`apps/web/tests/scaffold.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/scaffold.ts) — Contains the exact code literal `dsh-settings` named by the note.
- [`apps/web/tests/default-model.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/default-model.e2e.ts) — Contains the exact code literal `dsh-settings` named by the note.
- [`apps/web/tests/declared-reasoning.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/declared-reasoning.e2e.ts) — Contains the exact code literal `dsh-settings` named by the note.

## How to read the implementation

1. Start with [`packages/settings/settings/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/settings/settings/src/index.ts) because it has the strongest evidence link to the note.
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

- Tags: `class/architecture`, `concern/boundary`, `concern/cancellation-timeout`, `concern/compatibility`, `concern/concurrency`, `concern/evidence`, `concern/human-control`, `concern/lifecycle`, `concern/ownership`, `concern/performance`, `concern/recovery`, `concern/schema-types`, `concern/simplification`, `domain/build-release`, `domain/configuration`, `domain/extensions`, `domain/llm`, `domain/observability`, `domain/protocols`, `domain/security`, `domain/storage`, `lifecycle/implemented`, `mechanism/generation`, `mechanism/policy`, `mechanism/registry`
- Aliases: `watch`, `active`, `deepEqualJson`, `cloneJsonShaped`, `ready`, `dsh-settings-file`, `dsh-settings`, `settings/updated`, `structuredClone`, `persistSection`, `reconcileFromDisk`, `<file>.lock`, `pendingTails`, `INVARIANT`
- Regex: `(?i)(watch|active|deepEqualJson|cloneJsonShaped|ready|dsh\-settings\-file|dsh\-settings|settings/updated)`

```bash
rg -n --pcre2 "(?i)(watch|active|deepEqualJson|cloneJsonShaped|ready|dsh\\-settings\\-file|dsh\\-settings|settings/updated)" source-deepseek-harness
rg -l --fixed-strings "class/architecture" 03-agent-context
```

## Connected notes

- **`source-link`** — [0073. user-settings seam (`ctx.settings`) and the file provider](0073-user-settings-seam-ctx-settings-and-the-file-provider.md): The source note links to this decision directly.
- **`source-link`** — [0083. credential boundaries, whole-snapshot requests, and atomic route registration](0083-credential-boundaries-whole-snapshot-requests-and-atomic-route-registrat.md): The source note links to this decision directly.
- **`shares-code-with`** — [0355. Broken presets are roster rows, not gaps](0355-broken-presets-are-roster-rows-not-gaps.md): Shares source implementation: `packages/settings/settings`, `packages/settings/settings/src/index.ts`.
- **`shares-code-with`** — [0096. Splitting the credential store from the user environment layer](0096-splitting-the-credential-store-from-the-user-environment-layer.md): Shares source implementation: `packages/credentials/credentials-local`, `packages/credentials/credentials-local/src/index.ts`.
- **`shares-code-with`** — [0510. Client Settings, Locale, and Theme layering](0510-client-settings-locale-and-theme-layering.md): Shares source implementation: `packages/settings/settings/src/index.ts`, `packages/settings/settings/src/types.ts`.
- **`same-design-pressure`** — [0511. Session projections and command lifecycle logging](0511-session-projections-and-command-lifecycle-logging.md): Shares design concerns: `concern/boundary`, `concern/compatibility`, `concern/concurrency`.
- **`same-design-pressure`** — [0131. Code Mode --- the model writes TypeScript against the tool registry](0131-code-mode-the-model-writes-typescript-against-the-tool-registry.md): Shares design concerns: `concern/boundary`, `concern/cancellation-timeout`, `concern/concurrency`.
- **`same-design-pressure`** — [0500. Keyless browser e2e lane for the web GUI](0500-keyless-browser-e2e-lane-for-the-web-gui.md): Shares design concerns: `concern/boundary`, `concern/cancellation-timeout`, `concern/compatibility`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0086-settings-write-path-integrity-and-observer-lifecycle.md`.
