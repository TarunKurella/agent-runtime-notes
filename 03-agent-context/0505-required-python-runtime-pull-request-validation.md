---
id: "dsh-note-0505"
title: "Required Python runtime pull-request validation"
status: "implemented"
class: "testing"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/testing/2026-08-12-required-python-runtime-pull-request-ci.md"
implementation_evidence: "high"
target_anchor: "repository tests and engineering policy"
tags:
  - "class/testing"
  - "concern/boundary"
  - "concern/cancellation-timeout"
  - "concern/concurrency"
  - "concern/evidence"
  - "concern/schema-types"
  - "domain/build-release"
  - "domain/extensions"
  - "domain/multi-agent"
  - "domain/protocols"
  - "domain/session-state"
  - "domain/storage"
  - "lifecycle/implemented"
  - "mechanism/projection"
aliases:
  - "python-runtime"
  - "node24-linux-x64"
  - "python/"
  - "Required Python runtime pull-request validation"
  - "testing"
  - "boundary"
  - "cancellation timeout"
  - "concurrency"
  - "evidence"
  - "schema types"
  - "build release"
  - "extensions"
  - "multi agent"
  - "protocols"
search_regex: "(?i)(python\\-runtime|node24\\-linux\\-x64|python/|Required[- ]Python[- ]runtime[- ]pull\\-request[- ]validation|testing|boundary|cancellation[- ]timeout|concurrency)"
---

# 0505. Required Python runtime pull-request validation — implementation context

## Open this when

Ordinary pull-request CI runs the complete Python SDK pytest suite against fake runtime peers, while Node snapshots exercise different clients and expected outputs. The real Python client, packaged JSON-RPC executable, executable-specific snapshot, release-shaped wheels, and clean installation meet only in the optional single-executable or Python release workflows. A runtime event change or closure change can therefore merge with a stale Python projection or broken wheel path and fail only when someone later builds a Python release candidate.

## Source decision

Every pull request has a required python-runtime job in CI. It calls the shared single-executable builder for node24-linux-x64 without a path filter and participates in all checks passed. The called workflow builds the real executable, runs all keyless Python full-turn and direct-binary scenarios including the committed executable snapshot, builds the SDK and runtime wheels, installs them into a clean virtual environment, checks the executable and native addon's GLIBC requirements, and runs the installed wheels in a manylinux 2.28 container.

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/testing/2026-08-12-required-python-runtime-pull-request-ci.md](../02-notes/implemented/testing/2026-08-12-required-python-runtime-pull-request-ci.md)
- Pinned source: [.agents/notes/implemented/testing/2026-08-12-required-python-runtime-pull-request-ci.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/testing/2026-08-12-required-python-runtime-pull-request-ci.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`.github/workflows/ci.yml`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.github/workflows/ci.yml) | repository automation | The source note names this file directly. | `named-file` |
| [`.github/workflows/build-exe-for-python-sdk.yml`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.github/workflows/build-exe-for-python-sdk.yml) | repository automation | The source note names this file directly. | `named-file` |

### Code constructs named by the note

The note names behavior or policy but no uniquely resolvable code declaration. Search by its aliases and inspect the listed package entry points.

### Tests and executable evidence

- [`scripts/ci-workflow.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/ci-workflow.spec.ts) — A test under the owning area exercises or imports `python-runtime`. A test under the owning area exercises or imports `node24-linux-x64`.

## How to read the implementation

1. Start with [`.github/workflows/ci.yml`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.github/workflows/ci.yml) because it has the strongest evidence link to the note.
2. Read public types and entry points before implementation details. Write down the contract in plain language.
3. Follow the named constructs into their callers and tests. Confirm which component can actually enforce the rule.
4. Reconstruct one failure path: cancellation, crash, retry, stale state, partial write, or incompatible provider input—whichever the note is protecting against.
5. Only then map the contract into the current Rust repository. Target paths must come from that repository, never from guesswork in this wiki.

## Rust harness port

**Likely integration point:** repository tests and engineering policy.

Preserve the behavior and ownership rule. Do not mechanically translate Cordis, TypeScript declaration merging, Node streams, or package topology into Rust.

Relevant standing rules:

- **`rust/append-log`** — Keep durable facts append-only and derive views from them. Separate the synchronous fact boundary from asynchronous persistence.
- **`rust/four-tool-surface`** — Preserve the four model tools: read, write, patch, and exec. Reuse internal capabilities without automatically exposing new model-facing tools.
- **`rust/newtype-ids`** — Represent identity domains with Rust newtypes; do not pass bare strings across session, call, turn, step, and agent boundaries.
- **`rust/process-discipline`** — Treat this as repository or release policy unless it changes a runtime contract. Do not add runtime machinery for a process-only lesson.
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

- Tags: `class/testing`, `concern/boundary`, `concern/cancellation-timeout`, `concern/concurrency`, `concern/evidence`, `concern/schema-types`, `domain/build-release`, `domain/extensions`, `domain/multi-agent`, `domain/protocols`, `domain/session-state`, `domain/storage`, `lifecycle/implemented`, `mechanism/projection`
- Aliases: `python-runtime`, `node24-linux-x64`, `python/`, `Required Python runtime pull-request validation`, `testing`, `boundary`, `cancellation timeout`, `concurrency`, `evidence`, `schema types`, `build release`, `extensions`, `multi agent`, `protocols`
- Regex: `(?i)(python\-runtime|node24\-linux\-x64|python/|Required[- ]Python[- ]runtime[- ]pull\-request[- ]validation|testing|boundary|cancellation[- ]timeout|concurrency)`

```bash
rg -n --pcre2 "(?i)(python\\-runtime|node24\\-linux\\-x64|python/|Required[- ]Python[- ]runtime[- ]pull\\-request[- ]validation|testing|boundary|cancellation[- ]timeout|concurrency)" source-deepseek-harness
rg -l --fixed-strings "class/testing" 03-agent-context
```

## Connected notes

- **`source-link`** — [0441. Python public publication workflow](0441-python-public-publication-workflow.md): The source note links to this decision directly.
- **`shares-code-with`** — [0431. Dual Wine and native Windows pull-request CI](0431-dual-wine-and-native-windows-pull-request-ci.md): Shares source implementation: `.github/workflows/ci.yml`, `scripts/ci-workflow.spec.ts`.
- **`shares-code-with`** — [0407. CI failover runbook --- hosted pools → in-house pool](0407-ci-failover-runbook-hosted-pools-in-house-pool.md): Shares source implementation: `.github/workflows/ci.yml`, `scripts/ci-workflow.spec.ts`.
- **`shares-code-with`** — [0404. Portable pull-request CI recovery boundary](0404-portable-pull-request-ci-recovery-boundary.md): Shares source implementation: `.github/workflows/ci.yml`, `scripts/ci-workflow.spec.ts`.
- **`shares-code-with`** — [0646. Wine-run Windows blocking gates on Linux runners](0646-wine-run-windows-blocking-gates-on-linux-runners.md): Shares source implementation: `.github/workflows/ci.yml`, `scripts/ci-workflow.spec.ts`.
- **`shares-code-with`** — [0637. Parallel GitHub CI gates](0637-parallel-github-ci-gates.md): Shares source implementation: `.github/workflows/ci.yml`.
- **`shares-code-with`** — [0399. Serial cross-platform CI reference](0399-serial-cross-platform-ci-reference.md): Shares source implementation: `.github/workflows/ci.yml`.
- **`shares-code-with`** — [0317. Isolate pnpm setup per GitHub Actions runner](0317-isolate-pnpm-setup-per-github-actions-runner.md): Shares source implementation: `.github/workflows/ci.yml`, `scripts/ci-workflow.spec.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0505-required-python-runtime-pull-request-validation.md`.
