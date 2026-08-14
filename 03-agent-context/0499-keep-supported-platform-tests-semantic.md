---
id: "dsh-note-0499"
title: "Keep supported-platform tests semantic"
status: "implemented"
class: "testing"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/testing/2026-07-22-cross-platform-test-fixtures.md"
implementation_evidence: "medium"
target_anchor: "repository tests and engineering policy"
tags:
  - "class/testing"
  - "concern/boundary"
  - "concern/compatibility"
  - "concern/discovery-routing"
  - "concern/evidence"
  - "concern/ownership"
  - "concern/trust"
  - "domain/build-release"
  - "domain/extensions"
  - "domain/filesystem"
  - "domain/llm"
  - "domain/protocols"
  - "domain/security"
  - "domain/shell-terminal"
  - "domain/testing"
  - "lifecycle/implemented"
  - "mechanism/policy"
  - "mechanism/projection"
  - "mechanism/streaming"
aliases:
  - "taskkill"
  - "fileURLToPath"
  - "taskkill /T /F"
  - "Keep supported-platform tests semantic"
  - "testing"
  - "boundary"
  - "compatibility"
  - "discovery routing"
  - "evidence"
  - "ownership"
  - "trust"
  - "build release"
  - "extensions"
  - "filesystem"
search_regex: "(?i)(taskkill|fileURLToPath|taskkill[- ]/T[- ]/F|Keep[- ]supported\\-platform[- ]tests[- ]semantic|testing|boundary|compatibility|discovery[- ]routing)"
---

# 0499. Keep supported-platform tests semantic — implementation context

## Open this when

The unit and coverage suites run on Windows, macOS, and Linux, but a platform-neutral behavior can be hidden behind a platform-specific fixture. Literal POSIX paths become drive-relative paths on Windows, a hosted file: URI can be a valid UNC path there, and child-pipe closure or event-loop scheduling does not settle at the same point on every host. POSIX-only filesystem states such as FIFOs, executable mode bits, and directory search bits have no direct Windows fixture.

## Source decision

Tests of platform-neutral behavior construct absolute paths and file: URIs with the host's node:path and node:url APIs, then assert native absolute output or stable workspace-relative output as the contract requires. Invalid-URI fixtures use encodings rejected by fileURLToPath() on every supported platform. Transport-failure tests inject the connection's message writer and deliver the same asynchronous write callback error that a real Node stream would report. The production writer still writes framed messages to child stdin.

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/testing/2026-07-22-cross-platform-test-fixtures.md](../02-notes/implemented/testing/2026-07-22-cross-platform-test-fixtures.md)
- Pinned source: [.agents/notes/implemented/testing/2026-07-22-cross-platform-test-fixtures.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/testing/2026-07-22-cross-platform-test-fixtures.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`packages/subprocess/subprocess-local/src/spawn.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/subprocess/subprocess-local/src/spawn.ts) | runtime implementation | Defines `taskkill`, a construct named by the note. | `symbol-definition` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `taskkill` | `const` | [`packages/subprocess/subprocess-local/src/spawn.ts:332`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/subprocess/subprocess-local/src/spawn.ts#L332) | `const taskkill = internals.taskkill ?? taskkillProcessTree` |

### Tests and executable evidence

- [`scripts/publint-all.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/publint-all.spec.ts) — A test under the owning area exercises or imports `fileURLToPath`.
- [`apps/web/tests/goal-bar.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/goal-bar.e2e.ts) — A test under the owning area exercises or imports `fileURLToPath`.
- [`scripts/client-tsconfig.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/client-tsconfig.spec.ts) — A test under the owning area exercises or imports `fileURLToPath`.
- [`scripts/oxlint-contract.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/oxlint-contract.spec.ts) — A test under the owning area exercises or imports `fileURLToPath`.
- [`scripts/install-lefthook.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/install-lefthook.spec.ts) — A test under the owning area exercises or imports `fileURLToPath`.
- [`apps/web/tests/vite-entry.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/vite-entry.e2e.ts) — A test under the owning area exercises or imports `fileURLToPath`.
- [`apps/web/tests/pwa-manifest.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/pwa-manifest.e2e.ts) — A test under the owning area exercises or imports `fileURLToPath`.
- [`apps/web/tests/default-model.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/default-model.e2e.ts) — A test under the owning area exercises or imports `fileURLToPath`.

## How to read the implementation

1. Start with [`packages/subprocess/subprocess-local/src/spawn.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/subprocess/subprocess-local/src/spawn.ts) because it has the strongest evidence link to the note.
2. Read public types and entry points before implementation details. Write down the contract in plain language.
3. Follow the named constructs into their callers and tests. Confirm which component can actually enforce the rule.
4. Reconstruct one failure path: cancellation, crash, retry, stale state, partial write, or incompatible provider input—whichever the note is protecting against.
5. Only then map the contract into the current Rust repository. Target paths must come from that repository, never from guesswork in this wiki.

## Rust harness port

**Likely integration point:** repository tests and engineering policy.

Preserve the behavior and ownership rule. Do not mechanically translate Cordis, TypeScript declaration merging, Node streams, or package topology into Rust.

Relevant standing rules:

- **`rust/capability-security`** — Make granted capabilities explicit. On the Windows no-admin path, probe support at runtime and fail closed rather than silently running unsandboxed.
- **`rust/four-tool-surface`** — Preserve the four model tools: read, write, patch, and exec. Reuse internal capabilities without automatically exposing new model-facing tools.
- **`rust/process-discipline`** — Treat this as repository or release policy unless it changes a runtime contract. Do not add runtime machinery for a process-only lesson.
- **`rust/provider-adapter`** — Keep provider wire quirks inside adapters; the loop and durable event vocabulary remain provider-neutral.
- **`rust/quickjs-boundary`** — QuickJS holds orchestration variables; Rust/Tokio owns async execution and exposes only explicitly granted capabilities.
- **`rust/tokio-cancellation`** — Thread one cancellation token through async work, close owned resources before reuse, and test cancellation at every await boundary.
- **`rust/trait-seam`** — Use a small trait at the capability boundary; keep policy and model-facing schemas outside the backend implementation.

### Smallest safe implementation slice

1. Express the semantic contract as Rust types and one owning boundary.
2. Add the smallest adapter or service needed by the existing loop; do not add a new model-facing tool unless the four-tool surface cannot express the job.
3. Record durable evidence before deriving summaries or UI state.
4. Add one happy-path test, one interruption test, and one replay/restart test where state survives a turn.
5. Keep the patch reversible until the source note's strongest failure case is proven in the Rust harness.

## Search handles

- Tags: `class/testing`, `concern/boundary`, `concern/compatibility`, `concern/discovery-routing`, `concern/evidence`, `concern/ownership`, `concern/trust`, `domain/build-release`, `domain/extensions`, `domain/filesystem`, `domain/llm`, `domain/protocols`, `domain/security`, `domain/shell-terminal`, `domain/testing`, `lifecycle/implemented`, `mechanism/policy`, `mechanism/projection`, `mechanism/streaming`
- Aliases: `taskkill`, `fileURLToPath`, `taskkill /T /F`, `Keep supported-platform tests semantic`, `testing`, `boundary`, `compatibility`, `discovery routing`, `evidence`, `ownership`, `trust`, `build release`, `extensions`, `filesystem`
- Regex: `(?i)(taskkill|fileURLToPath|taskkill[- ]/T[- ]/F|Keep[- ]supported\-platform[- ]tests[- ]semantic|testing|boundary|compatibility|discovery[- ]routing)`

```bash
rg -n --pcre2 "(?i)(taskkill|fileURLToPath|taskkill[- ]/T[- ]/F|Keep[- ]supported\\-platform[- ]tests[- ]semantic|testing|boundary|compatibility|discovery[- ]routing)" source-deepseek-harness
rg -l --fixed-strings "class/testing" 03-agent-context
```

## Connected notes

- **`shares-code-with`** — [0372. Resolve Microsoft Store pwsh aliases](0372-resolve-microsoft-store-pwsh-aliases.md): Shares source implementation: `scripts/client-tsconfig.spec.ts`, `scripts/install-lefthook.spec.ts`.
- **`shares-code-with`** — [0431. Dual Wine and native Windows pull-request CI](0431-dual-wine-and-native-windows-pull-request-ci.md): Shares source implementation: `scripts/install-lefthook.spec.ts`.
- **`shares-code-with`** — [0421. Coverage-exempt heavy suites](0421-coverage-exempt-heavy-suites.md): Shares source implementation: `scripts/install-lefthook.spec.ts`, `scripts/oxlint-contract.spec.ts`.
- **`shares-code-with`** — [0415. Make Lefthook installation worktree-local](0415-make-lefthook-installation-worktree-local.md): Shares source implementation: `scripts/install-lefthook.spec.ts`, `scripts/oxlint-contract.spec.ts`.
- **`shares-code-with`** — [0320. The approval takeover shares the composer's text cap](0320-the-approval-takeover-shares-the-composer-s-text-cap.md): Shares source implementation: `scripts/install-lefthook.spec.ts`, `scripts/publint-all.spec.ts`.
- **`shares-code-with`** — [0411. Provision CI pnpm via pnpm/action-setup](0411-provision-ci-pnpm-via-pnpm-action-setup.md): Shares source implementation: `scripts/install-lefthook.spec.ts`, `scripts/oxlint-contract.spec.ts`.
- **`shares-code-with`** — [0373. Unlink fixture junctions before recursive deletion](0373-unlink-fixture-junctions-before-recursive-deletion.md): Shares source implementation: `scripts/oxlint-contract.spec.ts`, `scripts/publint-all.spec.ts`.
- **`shares-code-with`** — [0327. The composer's two text layers share one scrollport](0327-the-composer-s-two-text-layers-share-one-scrollport.md): Shares source implementation: `scripts/install-lefthook.spec.ts`, `scripts/publint-all.spec.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0499-keep-supported-platform-tests-semantic.md`.
