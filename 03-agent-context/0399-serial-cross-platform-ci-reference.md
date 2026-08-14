---
id: "dsh-note-0399"
title: "Serial cross-platform CI reference"
status: "implemented"
class: "process"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/process/2026-07-21-serial-cross-platform-ci-reference.md"
implementation_evidence: "high"
target_anchor: "repository tests and engineering policy"
tags:
  - "class/process"
  - "concern/boundary"
  - "concern/cancellation-timeout"
  - "concern/compatibility"
  - "concern/concurrency"
  - "concern/discovery-routing"
  - "concern/evidence"
  - "concern/lifecycle"
  - "concern/ownership"
  - "concern/performance"
  - "concern/schema-types"
  - "concern/trust"
  - "domain/build-release"
  - "domain/filesystem"
  - "domain/jobs-tasks"
  - "domain/llm"
  - "domain/observability"
  - "domain/protocols"
  - "domain/security"
  - "domain/session-state"
  - "domain/shell-terminal"
  - "domain/storage"
  - "domain/testing"
  - "lifecycle/implemented"
  - "mechanism/event-sourcing"
  - "mechanism/policy"
  - "mechanism/projection"
aliases:
  - "serial / linux"
  - "vm-backup"
  - "serial / windows"
  - "dsh-win-ci"
  - "serial / macos"
  - "TODO"
  - "workflow_dispatch"
  - "DSH_GATE_CONCURRENCY=1"
  - "terminal-bash"
  - "timeout-minutes"
  - "ubuntu-latest"
  - "macos-latest"
  - "windows-2025"
  - "dsh-windows-2025-16core"
search_regex: "(?i)(serial[- ]/[- ]linux|vm\\-backup|serial[- ]/[- ]windows|dsh\\-win\\-ci|serial[- ]/[- ]macos|TODO|workflow_dispatch|DSH_GATE_CONCURRENCY=1)"
---

# 0399. Serial cross-platform CI reference — implementation context

## Open this when

The pull-request workflow consolidates required checks into dedicated Linux and Windows jobs. Those jobs still should not be the only completeness oracle: a defect in their gate inventory or dependency graph could omit work while the required aggregate stays green. Encoding the one-minute non-Windows target and three-minute Windows target as job timeouts creates a separate failure mode. Hosted-runner startup and performance vary, so a correct gate can be cancelled at the target boundary before it emits useful diagnostics.

## Source decision

CI gives pull-request and master-push events complementary responsibilities. Pull requests run consolidated Linux and Wine-hosted Windows jobs plus the Node compatibility and Python contracts on standard GitHub-hosted capacity; an independent native Windows job reports the complete Windows inventory without participating in the required aggregate.

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/process/2026-07-21-serial-cross-platform-ci-reference.md](../02-notes/implemented/process/2026-07-21-serial-cross-platform-ci-reference.md)
- Pinned source: [.agents/notes/implemented/process/2026-07-21-serial-cross-platform-ci-reference.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/process/2026-07-21-serial-cross-platform-ci-reference.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`.github/workflows/ci.yml`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.github/workflows/ci.yml) | repository automation | The source note names this file directly. Contains the exact code literal `dsh-win-ci` named by the note. | `exact-code-occurrence, named-file` |
| [`.github/workflows/sandbox.yml`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.github/workflows/sandbox.yml) | repository automation | The source note names this file directly. | `named-file` |
| [`packages/terminal/terminal-bash/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/terminal/terminal-bash/src/index.ts) | package entry point | Core file in the package named by the note: `packages/terminal/terminal-bash`. | `named-package-member` |
| [`packages/terminal/terminal-bash/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/terminal/terminal-bash/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/terminal/terminal-bash`. | `named-package-member` |
| [`packages/terminal/terminal-bash`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/packages/terminal/terminal-bash) | package or module directory | The note names this package or capability. | `named-package` |
| [`packages/terminal/terminal-bash/README.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/terminal/terminal-bash/README.md) | package contract and examples | Core file in the package named by the note: `packages/terminal/terminal-bash`. | `named-package-member` |
| [`packages/terminal/terminal-bash/package.json`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/terminal/terminal-bash/package.json) | composition and configuration | Core file in the package named by the note: `packages/terminal/terminal-bash`. | `named-package-member` |
| [`.github/AGENTS.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.github/AGENTS.md) | repository automation | Contains the exact code literal `dsh-win-ci` named by the note. | `exact-code-occurrence` |

### Code constructs named by the note

The note names behavior or policy but no uniquely resolvable code declaration. Search by its aliases and inspect the listed package entry points.

### Tests and executable evidence

- [`packages/terminal/terminal-bash/tests/index.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/terminal/terminal-bash/tests/index.spec.ts) — A test under the owning area exercises or imports `terminal-bash`.
- [`packages/terminal/terminal-bash/tests/local.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/terminal/terminal-bash/tests/local.spec.ts) — A test under the owning area exercises or imports `terminal-bash`.
- [`packages/terminal/terminal-bash/tests/config.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/terminal/terminal-bash/tests/config.spec.ts) — A test under the owning area exercises or imports `terminal-bash`.
- [`scripts/ci-workflow.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/ci-workflow.spec.ts) — Contains the exact code literal `dsh-win-ci` named by the note. Contains the exact code literal `dsh-windows-2025-16core` named by the note.

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
- **`rust/capability-security`** — Make granted capabilities explicit. On the Windows no-admin path, probe support at runtime and fail closed rather than silently running unsandboxed.
- **`rust/four-tool-surface`** — Preserve the four model tools: read, write, patch, and exec. Reuse internal capabilities without automatically exposing new model-facing tools.
- **`rust/newtype-ids`** — Represent identity domains with Rust newtypes; do not pass bare strings across session, call, turn, step, and agent boundaries.
- **`rust/process-discipline`** — Treat this as repository or release policy unless it changes a runtime contract. Do not add runtime machinery for a process-only lesson.
- **`rust/provider-adapter`** — Keep provider wire quirks inside adapters; the loop and durable event vocabulary remain provider-neutral.
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

- Tags: `class/process`, `concern/boundary`, `concern/cancellation-timeout`, `concern/compatibility`, `concern/concurrency`, `concern/discovery-routing`, `concern/evidence`, `concern/lifecycle`, `concern/ownership`, `concern/performance`, `concern/schema-types`, `concern/trust`, `domain/build-release`, `domain/filesystem`, `domain/jobs-tasks`, `domain/llm`, `domain/observability`, `domain/protocols`, `domain/security`, `domain/session-state`, `domain/shell-terminal`, `domain/storage`, `domain/testing`, `lifecycle/implemented`, `mechanism/event-sourcing`, `mechanism/policy`, `mechanism/projection`
- Aliases: `serial / linux`, `vm-backup`, `serial / windows`, `dsh-win-ci`, `serial / macos`, `TODO`, `workflow_dispatch`, `DSH_GATE_CONCURRENCY=1`, `terminal-bash`, `timeout-minutes`, `ubuntu-latest`, `macos-latest`, `windows-2025`, `dsh-windows-2025-16core`
- Regex: `(?i)(serial[- ]/[- ]linux|vm\-backup|serial[- ]/[- ]windows|dsh\-win\-ci|serial[- ]/[- ]macos|TODO|workflow_dispatch|DSH_GATE_CONCURRENCY=1)`

```bash
rg -n --pcre2 "(?i)(serial[- ]/[- ]linux|vm\\-backup|serial[- ]/[- ]windows|dsh\\-win\\-ci|serial[- ]/[- ]macos|TODO|workflow_dispatch|DSH_GATE_CONCURRENCY=1)" source-deepseek-harness
rg -l --fixed-strings "class/process" 03-agent-context
```

## Connected notes

- **`source-link`** — [0404. Portable pull-request CI recovery boundary](0404-portable-pull-request-ci-recovery-boundary.md): The source note links to this decision directly.
- **`source-link`** — [0407. CI failover runbook --- hosted pools → in-house pool](0407-ci-failover-runbook-hosted-pools-in-house-pool.md): The source note links to this decision directly.
- **`source-link`** — [0431. Dual Wine and native Windows pull-request CI](0431-dual-wine-and-native-windows-pull-request-ci.md): The source note links to this decision directly.
- **`shares-code-with`** — [0496. Real-API e2e in CI against the external DeepSeek API](0496-real-api-e2e-in-ci-against-the-external-deepseek-api.md): Shares source implementation: `.github/workflows/ci.yml`.
- **`shares-code-with`** — [0637. Parallel GitHub CI gates](0637-parallel-github-ci-gates.md): Shares source implementation: `.github/workflows/ci.yml`.
- **`same-design-pressure`** — [0542. Dependency swaps rejected by the 2026-07 NIH audit](0542-dependency-swaps-rejected-by-the-2026-07-nih-audit.md): Shares design concerns: `concern/boundary`, `concern/cancellation-timeout`, `concern/compatibility`.
- **`same-design-pressure`** — [0158. persistent PTY sessions](0158-persistent-pty-sessions.md): Shares design concerns: `concern/boundary`, `concern/cancellation-timeout`, `concern/compatibility`.
- **`same-design-pressure`** — [0040. LSP capability seam and model-facing query tool](0040-lsp-capability-seam-and-model-facing-query-tool.md): Shares design concerns: `concern/boundary`, `concern/cancellation-timeout`, `concern/compatibility`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0399-serial-cross-platform-ci-reference.md`.
