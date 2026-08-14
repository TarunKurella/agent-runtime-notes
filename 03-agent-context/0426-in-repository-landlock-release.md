---
id: "dsh-note-0426"
title: "In-repository Landlock release"
status: "implemented"
class: "process"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/process/2026-08-06-in-repository-landlock-release.md"
implementation_evidence: "medium"
target_anchor: "repository tests and engineering policy"
tags:
  - "class/process"
  - "concern/boundary"
  - "concern/compatibility"
  - "concern/concurrency"
  - "concern/discovery-routing"
  - "concern/evidence"
  - "concern/human-control"
  - "concern/ownership"
  - "concern/recovery"
  - "concern/trust"
  - "domain/build-release"
  - "domain/filesystem"
  - "domain/protocols"
  - "domain/security"
  - "domain/testing"
  - "lifecycle/implemented"
  - "mechanism/generation"
  - "mechanism/registry"
aliases:
  - "cpu"
  - "next"
  - "@deepseek-ai/node-addon-landlock-run"
  - "native/landlock-run"
  - "@deepseek-ai"
  - "native/landlock-run/packages/*"
  - "pnpm-lock.yaml"
  - "lib/"
  - "@deepseek-ai/node-addon-landlock-run-linux-x64"
  - "@deepseek-ai/node-addon-landlock-run-linux-arm64"
  - "optionalDependencies"
  - "publishConfig.access: public"
  - "Landlock Run"
  - "Landlock Run Release"
search_regex: "(?i)(next|@deepseek\\-ai/node\\-addon\\-landlock\\-run|native/landlock\\-run|@deepseek\\-ai|native/landlock\\-run/packages/\\*|pnpm\\-lock\\.yaml|lib/|@deepseek\\-ai/node\\-addon\\-landlock\\-run\\-linux\\-x64)"
---

# 0426. In-repository Landlock release — implementation context

## Open this when

The @deepseek-ai/node-addon-landlock-run source already lives beside its DeepSeek Harness consumers under native/landlock-run, but it previously kept a separate pnpm workspace and lockfile and depended on a standalone repository for npm publication. Harness packages consumed a fixed registry version, so one pull request could change the launcher contract and its consumer without testing those changes together. The source repository's native workflow could rehearse the package, but it did not publish the artifact it tested.

## Source decision

native/landlock-run and native/landlock-run/packages/ belong to the repository's root pnpm workspace and use the root pnpm-lock.yaml. Harness consumers declare @deepseek-ai/node-addon-landlock-run with workspace:, so development, type checking, builds, and pull-request tests resolve the entry package from the same checkout. The root TypeScript project graph builds that entry package before consumers, and the repository cleaner owns its direct lib/ output.

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/process/2026-08-06-in-repository-landlock-release.md](../02-notes/implemented/process/2026-08-06-in-repository-landlock-release.md)
- Pinned source: [.agents/notes/implemented/process/2026-08-06-in-repository-landlock-release.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/process/2026-08-06-in-repository-landlock-release.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`native/landlock-run/packages/entry/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/native/landlock-run/packages/entry/src/index.ts) | package entry point | Core file in the package named by the note: `native/landlock-run/packages/entry`. | `named-package-member` |
| [`native/landlock-run/README.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/native/landlock-run/README.md) | package contract and examples | Entry point or contract under the directory named by the note: `native/landlock-run`. | `named-directory-member` |
| [`native/landlock-run/package.json`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/native/landlock-run/package.json) | composition and configuration | Entry point or contract under the directory named by the note: `native/landlock-run`. | `named-directory-member` |
| [`native/landlock-run/scripts/repo.mjs`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/native/landlock-run/scripts/repo.mjs) | runtime implementation | Entry point or contract under the directory named by the note: `native/landlock-run`. Defines `cpu`, a construct named by the note. | `named-directory-member, symbol-definition` |
| [`native/landlock-run`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/native/landlock-run) | package or module directory | The source note names this implementation area directly. | `named-directory` |
| [`native/landlock-run/packages`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/native/landlock-run/packages) | package or module directory | The source note names this implementation area directly. | `named-directory` |
| [`native/landlock-run/packages/entry`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/native/landlock-run/packages/entry) | package or module directory | The note names this package or capability. | `named-package` |
| [`native/landlock-run/packages/linux-x64`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/native/landlock-run/packages/linux-x64) | package or module directory | The note names this package or capability. | `named-package` |
| [`native/landlock-run/packages/linux-arm64`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/native/landlock-run/packages/linux-arm64) | package or module directory | The note names this package or capability. | `named-package` |
| [`vendor/loader/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/loader/src/index.ts) | package entry point | Defines `next`, a construct named by the note. | `symbol-definition` |
| [`native/landlock-run/packages/entry/README.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/native/landlock-run/packages/entry/README.md) | package contract and examples | Core file in the package named by the note: `native/landlock-run/packages/entry`. | `named-package-member` |
| [`native/landlock-run/packages/entry/package.json`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/native/landlock-run/packages/entry/package.json) | composition and configuration | Core file in the package named by the note: `native/landlock-run/packages/entry`. | `named-package-member` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `cpu` | `const` | [`native/landlock-run/scripts/repo.mjs:55`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/native/landlock-run/scripts/repo.mjs#L55) | `const cpu = manifest.cpu?.[0];` |
| `next` | `const` | [`vendor/loader/src/index.ts:181`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/loader/src/index.ts#L181) | `const next = fiber.parent.fiber` |

### Tests and executable evidence

- [`scripts/gen-third-party-notices.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/gen-third-party-notices.spec.ts) — Contains the exact code literal `native/landlock-run` named by the note.

## How to read the implementation

1. Start with [`native/landlock-run/packages/entry/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/native/landlock-run/packages/entry/src/index.ts) because it has the strongest evidence link to the note.
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
- **`rust/trait-seam`** — Use a small trait at the capability boundary; keep policy and model-facing schemas outside the backend implementation.

### Smallest safe implementation slice

1. Express the semantic contract as Rust types and one owning boundary.
2. Add the smallest adapter or service needed by the existing loop; do not add a new model-facing tool unless the four-tool surface cannot express the job.
3. Record durable evidence before deriving summaries or UI state.
4. Add one happy-path test, one interruption test, and one replay/restart test where state survives a turn.
5. Keep the patch reversible until the source note's strongest failure case is proven in the Rust harness.

## Search handles

- Tags: `class/process`, `concern/boundary`, `concern/compatibility`, `concern/concurrency`, `concern/discovery-routing`, `concern/evidence`, `concern/human-control`, `concern/ownership`, `concern/recovery`, `concern/trust`, `domain/build-release`, `domain/filesystem`, `domain/protocols`, `domain/security`, `domain/testing`, `lifecycle/implemented`, `mechanism/generation`, `mechanism/registry`
- Aliases: `cpu`, `next`, `@deepseek-ai/node-addon-landlock-run`, `native/landlock-run`, `@deepseek-ai`, `native/landlock-run/packages/*`, `pnpm-lock.yaml`, `lib/`, `@deepseek-ai/node-addon-landlock-run-linux-x64`, `@deepseek-ai/node-addon-landlock-run-linux-arm64`, `optionalDependencies`, `publishConfig.access: public`, `Landlock Run`, `Landlock Run Release`
- Regex: `(?i)(next|@deepseek\-ai/node\-addon\-landlock\-run|native/landlock\-run|@deepseek\-ai|native/landlock\-run/packages/\*|pnpm\-lock\.yaml|lib/|@deepseek\-ai/node\-addon\-landlock\-run\-linux\-x64)`

```bash
rg -n --pcre2 "(?i)(next|@deepseek\\-ai/node\\-addon\\-landlock\\-run|native/landlock\\-run|@deepseek\\-ai|native/landlock\\-run/packages/\\*|pnpm\\-lock\\.yaml|lib/|@deepseek\\-ai/node\\-addon\\-landlock\\-run\\-linux\\-x64)" source-deepseek-harness
rg -l --fixed-strings "class/process" 03-agent-context
```

## Connected notes

- **`source-link`** — [0146. The subprocess sandbox --- confinement seam, native runners, escalation, and per-session modes](0146-the-subprocess-sandbox-confinement-seam-native-runners-escalation-and-pe.md): The source note links to this decision directly.
- **`source-link`** — [0444. npm access per release sequence: the vendored framework and the native packages publish publicly](0444-npm-access-per-release-sequence-the-vendored-framework-and-the-native-pa.md): The source note links to this decision directly.
- **`source-link`** — [0527. Artifact-first NPM baseline publication](0527-artifact-first-npm-baseline-publication.md): The source note links to this decision directly.
- **`shares-code-with`** — [0413. Dependabot version updates with a 30-day cooldown](0413-dependabot-version-updates-with-a-30-day-cooldown.md): Shares source implementation: `native/landlock-run`, `native/landlock-run/README.md`.
- **`shares-code-with`** — [0532. Evaluate landstrip before building a Windows sandbox launcher](0532-evaluate-landstrip-before-building-a-windows-sandbox-launcher.md): Shares source implementation: `native/landlock-run/packages/entry`, `native/landlock-run/packages/entry/src/index.ts`.
- **`shares-code-with`** — [0439. Private npm publication as three independent sequences](0439-private-npm-publication-as-three-independent-sequences.md): Shares source implementation: `vendor/loader/src/index.ts`.
- **`shares-code-with`** — [0044. GUI layering and the RPC protocol --- host/client layering by capability provider, the four-quadrant message model, and the fetch carrier](0044-gui-layering-and-the-rpc-protocol-host-client-layering-by-capability-pro.md): Shares source implementation: `vendor/loader/src/index.ts`.
- **`shares-code-with`** — [0045. Web client architecture --- the client cordis plugin tree, the slot system, and the React-free object layer](0045-web-client-architecture-the-client-cordis-plugin-tree-the-slot-system-an.md): Shares source implementation: `vendor/loader/src/index.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0426-in-repository-landlock-release.md`.
