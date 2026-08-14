---
id: "dsh-note-0413"
title: "Dependabot version updates with a 30-day cooldown"
status: "implemented"
class: "process"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/process/2026-07-27-dependabot-version-updates.md"
implementation_evidence: "high"
target_anchor: "repository tests and engineering policy"
tags:
  - "class/process"
  - "concern/boundary"
  - "concern/compatibility"
  - "concern/discovery-routing"
  - "concern/evidence"
  - "concern/ownership"
  - "concern/simplification"
  - "concern/trust"
  - "domain/build-release"
  - "domain/configuration"
  - "domain/extensions"
  - "domain/filesystem"
  - "domain/llm"
  - "domain/security"
  - "domain/testing"
  - "lifecycle/implemented"
  - "mechanism/policy"
  - "mechanism/registry"
aliases:
  - ".github/dependabot.yml"
  - "native/landlock-run"
  - "python/sdk"
  - "cooldown.default-days"
  - "vendor/**"
  - "exclude-paths"
  - "kind/dependency"
  - "area/infra"
  - "packageManager"
  - "9.0"
  - "Dependabot version updates with a 30-day cooldown"
  - "process"
  - "boundary"
  - "compatibility"
search_regex: "(?i)(\\.github/dependabot\\.yml|native/landlock\\-run|python/sdk|cooldown\\.default\\-days|vendor/\\*\\*|exclude\\-paths|kind/dependency|area/infra)"
---

# 0413. Dependabot version updates with a 30-day cooldown — implementation context

## Open this when

Maintained registry and GitHub Actions dependencies need a regular update path. Adopting every release immediately increases exposure to compromised releases and early regressions, while leaving updates entirely manual lets dependency drift accumulate. Vendored Cordis sources cannot be treated like registry dependencies, and workspaces sharing one lockfile must be updated through the same package tree.

## Source decision

The default branch carries .github/dependabot.yml with weekly version-update checks for the root pnpm workspace, including native/landlock-run, the python/sdk uv project, and GitHub Actions. Every entry sets cooldown.default-days to 30, so a version release becomes eligible only after it is at least 30 days old and is proposed on the next weekly check. The in-repository Landlock release decision owns the shared-workspace boundary. The root pnpm version-update scan excludes vendor/, whose source and manifests move only through the vendoring procedure.

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/process/2026-07-27-dependabot-version-updates.md](../02-notes/implemented/process/2026-07-27-dependabot-version-updates.md)
- Pinned source: [.agents/notes/implemented/process/2026-07-27-dependabot-version-updates.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/process/2026-07-27-dependabot-version-updates.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`vendor/README.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/README.md) | package contract and examples | The source note names this file directly. | `named-file` |
| [`.github/dependabot.yml`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.github/dependabot.yml) | repository automation | The source note names this file directly. Contains the exact code literal `python/sdk` named by the note. | `exact-code-occurrence, named-file` |
| [`python/sdk/README.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/python/sdk/README.md) | package contract and examples | Entry point or contract under the directory named by the note: `python/sdk`. | `named-directory-member` |
| [`native/landlock-run/README.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/native/landlock-run/README.md) | package contract and examples | Entry point or contract under the directory named by the note: `native/landlock-run`. | `named-directory-member` |
| [`native/landlock-run/package.json`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/native/landlock-run/package.json) | composition and configuration | Entry point or contract under the directory named by the note: `native/landlock-run`. | `named-directory-member` |
| [`python/sdk`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/python/sdk) | package or module directory | The source note names this implementation area directly. | `named-directory` |
| [`native/landlock-run`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/native/landlock-run) | package or module directory | The source note names this implementation area directly. | `named-directory` |
| [`package.json`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/package.json) | composition and configuration | Contains the exact code literal `native/landlock-run` named by the note. | `exact-code-occurrence` |
| [`pnpm-lock.yaml`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/pnpm-lock.yaml) | composition and configuration | Contains the exact code literal `native/landlock-run` named by the note. | `exact-code-occurrence` |
| [`.gitlab-ci.yml`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.gitlab-ci.yml) | composition and configuration | Contains the exact code literal `python/sdk` named by the note. | `exact-code-occurrence` |
| [`pnpm-workspace.yaml`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/pnpm-workspace.yaml) | composition and configuration | Contains the exact code literal `native/landlock-run` named by the note. | `exact-code-occurrence` |
| [`python/development.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/python/development.md) | package contract and examples | Contains the exact code literal `python/sdk` named by the note. | `exact-code-occurrence` |

### Code constructs named by the note

The note names behavior or policy but no uniquely resolvable code declaration. Search by its aliases and inspect the listed package entry points.

### Tests and executable evidence

- [`scripts/gen-third-party-notices.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/gen-third-party-notices.spec.ts) — Contains the exact code literal `native/landlock-run` named by the note.
- [`.github/issue-management/policy.test.mjs`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.github/issue-management/policy.test.mjs) — Contains the exact code literal `kind/dependency` named by the note. Contains the exact code literal `area/infra` named by the note.
- [`python/sdk/tests/manual_sdk_agent_smoke.py`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/python/sdk/tests/manual_sdk_agent_smoke.py) — Contains the exact code literal `python/sdk` named by the note.

## How to read the implementation

1. Start with [`vendor/README.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/README.md) because it has the strongest evidence link to the note.
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
- **`rust/trait-seam`** — Use a small trait at the capability boundary; keep policy and model-facing schemas outside the backend implementation.

### Smallest safe implementation slice

1. Express the semantic contract as Rust types and one owning boundary.
2. Add the smallest adapter or service needed by the existing loop; do not add a new model-facing tool unless the four-tool surface cannot express the job.
3. Record durable evidence before deriving summaries or UI state.
4. Add one happy-path test, one interruption test, and one replay/restart test where state survives a turn.
5. Keep the patch reversible until the source note's strongest failure case is proven in the Rust harness.

## Search handles

- Tags: `class/process`, `concern/boundary`, `concern/compatibility`, `concern/discovery-routing`, `concern/evidence`, `concern/ownership`, `concern/simplification`, `concern/trust`, `domain/build-release`, `domain/configuration`, `domain/extensions`, `domain/filesystem`, `domain/llm`, `domain/security`, `domain/testing`, `lifecycle/implemented`, `mechanism/policy`, `mechanism/registry`
- Aliases: `.github/dependabot.yml`, `native/landlock-run`, `python/sdk`, `cooldown.default-days`, `vendor/**`, `exclude-paths`, `kind/dependency`, `area/infra`, `packageManager`, `9.0`, `Dependabot version updates with a 30-day cooldown`, `process`, `boundary`, `compatibility`
- Regex: `(?i)(\.github/dependabot\.yml|native/landlock\-run|python/sdk|cooldown\.default\-days|vendor/\*\*|exclude\-paths|kind/dependency|area/infra)`

```bash
rg -n --pcre2 "(?i)(\\.github/dependabot\\.yml|native/landlock\\-run|python/sdk|cooldown\\.default\\-days|vendor/\\*\\*|exclude\\-paths|kind/dependency|area/infra)" source-deepseek-harness
rg -l --fixed-strings "class/process" 03-agent-context
```

## Connected notes

- **`source-link`** — [0426. In-repository Landlock release](0426-in-repository-landlock-release.md): The source note links to this decision directly.
- **`shares-code-with`** — [0439. Private npm publication as three independent sequences](0439-private-npm-publication-as-three-independent-sequences.md): Shares source implementation: `vendor/README.md`.
- **`shares-code-with`** — [0467. One shared base config with per-surface overlays](0467-one-shared-base-config-with-per-surface-overlays.md): Shares source implementation: `vendor/README.md`.
- **`shares-code-with`** — [0034. Single-file executable SDK runtime distribution (single-exe)](0034-single-file-executable-sdk-runtime-distribution-single-exe.md): Shares source implementation: `.gitlab-ci.yml`.
- **`shares-code-with`** — [0419. Generated third-party notices](0419-generated-third-party-notices.md): Shares source implementation: `vendor/README.md`.
- **`shares-code-with`** — [0676. Explicit-config dsh entrypoint](0676-explicit-config-dsh-entrypoint.md): Shares source implementation: `pnpm-lock.yaml`.
- **`shares-code-with`** — [0415. Make Lefthook installation worktree-local](0415-make-lefthook-installation-worktree-local.md): Shares source implementation: `package.json`.
- **`shares-code-with`** — [0120. Product subagent providers live in the shared profile host](0120-product-subagent-providers-live-in-the-shared-profile-host.md): Shares source implementation: `pnpm-lock.yaml`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0413-dependabot-version-updates-with-a-30-day-cooldown.md`.
