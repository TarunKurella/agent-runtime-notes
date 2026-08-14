---
id: "dsh-note-0378"
title: "Vendor Cordis as source, not npm dependencies"
status: "implemented"
class: "process"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/process/2026-06-11-vendor-cordis-as-source.md"
implementation_evidence: "high"
target_anchor: "repository tests and engineering policy"
tags:
  - "class/process"
  - "concern/discovery-routing"
  - "concern/evidence"
  - "concern/lifecycle"
  - "concern/schema-types"
  - "domain/agent-loop"
  - "domain/build-release"
  - "domain/extensions"
  - "domain/filesystem"
  - "lifecycle/implemented"
  - "mechanism/generation"
  - "mechanism/policy"
aliases:
  - "vendor/"
  - "pnpm-workspace.yaml"
  - "linkWorkspacePackages: true"
  - "vendor/README.md"
  - "scripts/check-vendor-manifest.sh"
  - "Vendor Cordis as source, not npm dependencies"
  - "process"
  - "discovery routing"
  - "evidence"
  - "lifecycle"
  - "schema types"
  - "agent loop"
  - "build release"
  - "extensions"
search_regex: "(?i)(vendor/|pnpm\\-workspace\\.yaml|linkWorkspacePackages:[- ]true|vendor/README\\.md|scripts/check\\-vendor\\-manifest\\.sh|Vendor[- ]Cordis[- ]as[- ]source,[- ]not[- ]npm[- ]dependencies|discovery[- ]routing|evidence)"
---

# 0378. Vendor Cordis as source, not npm dependencies — implementation context

## Open this when

DeepSeek Harness is built on the Cordis framework. Cordis core was at 4.0.0-rc.6 (a release candidate) when this repo started; the harness depends on framework internals (fiber lifecycle, effect disposal, waterfall dispatch) whose exact behavior matters to the agent loop's correctness guarantees.

## Source decision

Copy the needed Cordis packages (core, loader, include, group, timer, hmr, logger-console) and the cordiverse foundation libraries (cosmokit, schemastery) into vendor/ as source, flattened, keeping their original npm names so workspace resolution is transparent. pnpm-workspace.yaml sets linkWorkspacePackages: true, so matching upstream semver ranges resolve these pinned workspaces in both source and built-artifact execution. Truly third-party dependencies (js-yaml, chokidar, @standard-schema/spec, …) stay on npm.

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/process/2026-06-11-vendor-cordis-as-source.md](../02-notes/implemented/process/2026-06-11-vendor-cordis-as-source.md)
- Pinned source: [.agents/notes/implemented/process/2026-06-11-vendor-cordis-as-source.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/process/2026-06-11-vendor-cordis-as-source.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`vendor/README.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/README.md) | package contract and examples | The source note names this file directly. | `named-file` |
| [`scripts/check-vendor-manifest.sh`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/check-vendor-manifest.sh) | repository automation | The source note names this file directly. | `named-file` |
| [`CLAUDE.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/CLAUDE.md) | package contract and examples | Contains the exact code literal `vendor/README.md` named by the note. | `exact-code-occurrence` |
| [`AGENTS.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/AGENTS.md) | package contract and examples | Contains the exact code literal `vendor/README.md` named by the note. | `exact-code-occurrence` |
| [`lefthook.yml`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/lefthook.yml) | composition and configuration | Contains the exact code literal `vendor/README.md` named by the note. Contains the exact code literal `scripts/check-vendor-manifest.sh` named by the note. | `exact-code-occurrence` |
| [`THIRD_PARTY_NOTICES.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/THIRD_PARTY_NOTICES.md) | package contract and examples | Contains the exact code literal `vendor/README.md` named by the note. | `exact-code-occurrence` |
| [`scripts/rescope-vendor.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/rescope-vendor.ts) | repository automation | Contains the exact code literal `vendor/README.md` named by the note. | `exact-code-occurrence` |
| [`scripts/publish-npm-baseline.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/publish-npm-baseline.ts) | repository automation | Contains the exact code literal `vendor/README.md` named by the note. | `exact-code-occurrence` |
| [`scripts/check-expected-filenames.sh`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/check-expected-filenames.sh) | repository automation | Contains the exact code literal `vendor/README.md` named by the note. | `exact-code-occurrence` |
| [`docs/cookbook/adding-a-vendored-package.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/cookbook/adding-a-vendored-package.md) | package contract and examples | Contains the exact code literal `scripts/check-vendor-manifest.sh` named by the note. | `exact-code-occurrence` |
| [`docs/cookbook/adding-a-vendored-package.zh.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/cookbook/adding-a-vendored-package.zh.md) | package contract and examples | Contains the exact code literal `scripts/check-vendor-manifest.sh` named by the note. | `exact-code-occurrence` |

### Code constructs named by the note

The note names behavior or policy but no uniquely resolvable code declaration. Search by its aliases and inspect the listed package entry points.

### Tests and executable evidence

- [`scripts/gen-third-party-notices.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/gen-third-party-notices.spec.ts) — Contains the exact code literal `vendor/README.md` named by the note.

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
- **`rust/newtype-ids`** — Represent identity domains with Rust newtypes; do not pass bare strings across session, call, turn, step, and agent boundaries.
- **`rust/process-discipline`** — Treat this as repository or release policy unless it changes a runtime contract. Do not add runtime machinery for a process-only lesson.
- **`rust/quickjs-boundary`** — QuickJS holds orchestration variables; Rust/Tokio owns async execution and exposes only explicitly granted capabilities.
- **`rust/serde-validation`** — Parse untrusted input into strict serde types and validate semantic constraints before executing side effects.
- **`rust/tagged-enums`** — Use serde-tagged enums for a closed wire vocabulary. Add an explicit extension envelope only where runtime extension is truly required.
- **`rust/trait-seam`** — Use a small trait at the capability boundary; keep policy and model-facing schemas outside the backend implementation.

### Smallest safe implementation slice

1. Express the semantic contract as Rust types and one owning boundary.
2. Add the smallest adapter or service needed by the existing loop; do not add a new model-facing tool unless the four-tool surface cannot express the job.
3. Record durable evidence before deriving summaries or UI state.
4. Add one happy-path test, one interruption test, and one replay/restart test where state survives a turn.
5. Keep the patch reversible until the source note's strongest failure case is proven in the Rust harness.

## Search handles

- Tags: `class/process`, `concern/discovery-routing`, `concern/evidence`, `concern/lifecycle`, `concern/schema-types`, `domain/agent-loop`, `domain/build-release`, `domain/extensions`, `domain/filesystem`, `lifecycle/implemented`, `mechanism/generation`, `mechanism/policy`
- Aliases: `vendor/`, `pnpm-workspace.yaml`, `linkWorkspacePackages: true`, `vendor/README.md`, `scripts/check-vendor-manifest.sh`, `Vendor Cordis as source, not npm dependencies`, `process`, `discovery routing`, `evidence`, `lifecycle`, `schema types`, `agent loop`, `build release`, `extensions`
- Regex: `(?i)(vendor/|pnpm\-workspace\.yaml|linkWorkspacePackages:[- ]true|vendor/README\.md|scripts/check\-vendor\-manifest\.sh|Vendor[- ]Cordis[- ]as[- ]source,[- ]not[- ]npm[- ]dependencies|discovery[- ]routing|evidence)`

```bash
rg -n --pcre2 "(?i)(vendor/|pnpm\\-workspace\\.yaml|linkWorkspacePackages:[- ]true|vendor/README\\.md|scripts/check\\-vendor\\-manifest\\.sh|Vendor[- ]Cordis[- ]as[- ]source,[- ]not[- ]npm[- ]dependencies|discovery[- ]routing|evidence)" source-deepseek-harness
rg -l --fixed-strings "class/process" 03-agent-context
```

## Connected notes

- **`shares-code-with`** — [0523. Supply chain checks and vendor drift verification](0523-supply-chain-checks-and-vendor-drift-verification.md): Shares source implementation: `AGENTS.md`, `CLAUDE.md`.
- **`shares-code-with`** — [0439. Private npm publication as three independent sequences](0439-private-npm-publication-as-three-independent-sequences.md): Shares source implementation: `scripts/publish-npm-baseline.ts`, `vendor/README.md`.
- **`shares-code-with`** — [0440. Rescope vendored Cordis into @deepseek-ai](0440-rescope-vendored-cordis-into-deepseek-ai.md): Shares source implementation: `scripts/rescope-vendor.ts`, `vendor/README.md`.
- **`shares-code-with`** — [0419. Generated third-party notices](0419-generated-third-party-notices.md): Shares source implementation: `THIRD_PARTY_NOTICES.md`, `vendor/README.md`.
- **`shares-code-with`** — [0430. Lightweight routine documentation translation](0430-lightweight-routine-documentation-translation.md): Shares source implementation: `AGENTS.md`, `CLAUDE.md`.
- **`shares-code-with`** — [0249. Claude Code and Codex subagent backends](0249-claude-code-and-codex-subagent-backends.md): Shares source implementation: `THIRD_PARTY_NOTICES.md`.
- **`shares-code-with`** — [0613. demo:web builds the client plugin bundles](0613-demo-web-builds-the-client-plugin-bundles.md): Shares source implementation: `AGENTS.md`, `CLAUDE.md`.
- **`shares-code-with`** — [0527. Artifact-first NPM baseline publication](0527-artifact-first-npm-baseline-publication.md): Shares source implementation: `scripts/publish-npm-baseline.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0378-vendor-cordis-as-source-not-npm-dependencies.md`.
