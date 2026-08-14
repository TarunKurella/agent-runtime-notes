---
id: "dsh-note-0532"
title: "Evaluate landstrip before building a Windows sandbox launcher"
status: "rejected"
class: "feature"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/rejected/feature/2026-07-26-evaluate-landstrip-for-windows-sandbox-rung.md"
implementation_evidence: "medium"
target_anchor: "sandbox capability boundary"
tags:
  - "class/feature"
  - "concern/boundary"
  - "concern/compatibility"
  - "concern/discovery-routing"
  - "concern/evidence"
  - "concern/lifecycle"
  - "concern/ownership"
  - "concern/trust"
  - "domain/build-release"
  - "domain/security"
  - "domain/testing"
  - "lifecycle/rejected"
  - "mechanism/policy"
  - "mechanism/registry"
aliases:
  - "PLATFORM_CHAINS.win32"
  - "node-addon-landlock-run"
  - "@landstrip/landstrip"
  - "optionalDependencies"
  - "--probe"
  - "Evaluate landstrip before building a Windows sandbox launcher"
  - "feature"
  - "boundary"
  - "compatibility"
  - "discovery routing"
  - "evidence"
  - "lifecycle"
  - "ownership"
  - "trust"
search_regex: "(?i)(PLATFORM_CHAINS\\.win32|node\\-addon\\-landlock\\-run|@landstrip/landstrip|optionalDependencies|\\-\\-probe|feature|boundary|compatibility)"
---

# 0532. Evaluate landstrip before building a Windows sandbox launcher — implementation context

## Open this when

The sandbox decision leaves PLATFORM_CHAINS.win32 empty and plans to fill it with "a confinement runner from the AppContainer/restricted-token family, shipped from its own repository on the node-addon-landlock-run template" --- an estimated ~1,500-line new repo (the landlock-run subtree is ~1,460 lines of C/TS/scripts/tests plus docs and CI) authored and maintained in-house.

## Source decision

When the Windows sandbox phase is picked up, evaluate wrapping landstrip's Windows backend as the win32 chain runner before authoring an in-house AppContainer launcher repository. The evaluation must answer: Probe synthesis. landstrip has no --probe; the chain's functional-probe contract would have to be synthesized from a trap run. Dialect mapping. Denial and runner-failure stderr dialects, and fail-closed exit-code classification, need explicit mapping into the chain's vocabulary. License. The binaries are LGPL-2.1-or-later; distribution review is required before it enters the shipped closure.

## Decision status

Rejected design. Do not implement it as written. Reuse its failure analysis, constraints, and reversal condition.

- Raw note: [rejected/feature/2026-07-26-evaluate-landstrip-for-windows-sandbox-rung.md](../02-notes/rejected/feature/2026-07-26-evaluate-landstrip-for-windows-sandbox-rung.md)
- Pinned source: [.agents/notes/rejected/feature/2026-07-26-evaluate-landstrip-for-windows-sandbox-rung.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/rejected/feature/2026-07-26-evaluate-landstrip-for-windows-sandbox-rung.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`native/landlock-run/packages/entry/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/native/landlock-run/packages/entry/src/index.ts) | package entry point | Core file in the package named by the note: `native/landlock-run/packages/entry`. | `named-package-member` |
| [`native/landlock-run/packages/entry`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/native/landlock-run/packages/entry) | package or module directory | The note names this package or capability. | `named-package` |
| [`native/landlock-run/packages/entry/README.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/native/landlock-run/packages/entry/README.md) | package contract and examples | Core file in the package named by the note: `native/landlock-run/packages/entry`. | `named-package-member` |
| [`native/landlock-run/packages/entry/package.json`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/native/landlock-run/packages/entry/package.json) | composition and configuration | Core file in the package named by the note: `native/landlock-run/packages/entry`. | `named-package-member` |

### Code constructs named by the note

The note names behavior or policy but no uniquely resolvable code declaration. Search by its aliases and inspect the listed package entry points.

### Tests and executable evidence

- No test file was tied to this note with enough confidence. Read the note's verification section and search the owning package's `tests/` directory.
- Source verification intent: Before any Windows-rung implementation starts, an evaluation records the probe, dialect, license, source repository, release process, and binary build answers, and the go/no-go is added to the sandbox note's deferred-phases plan.

## How to read the implementation

1. Start with [`native/landlock-run/packages/entry/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/native/landlock-run/packages/entry/src/index.ts) because it has the strongest evidence link to the note.
2. Read public types and entry points before implementation details. Write down the contract in plain language.
3. Follow the named constructs into their callers and tests. Confirm which component can actually enforce the rule.
4. Reconstruct one failure path: cancellation, crash, retry, stale state, partial write, or incompatible provider input—whichever the note is protecting against.
5. Only then map the contract into the current Rust repository. Target paths must come from that repository, never from guesswork in this wiki.

## Rust harness port

**Likely integration point:** sandbox capability boundary.

Preserve the behavior and ownership rule. Do not mechanically translate Cordis, TypeScript declaration merging, Node streams, or package topology into Rust.

Relevant standing rules:

- **`rust/capability-security`** — Make granted capabilities explicit. On the Windows no-admin path, probe support at runtime and fail closed rather than silently running unsandboxed.
- **`rust/four-tool-surface`** — Preserve the four model tools: read, write, patch, and exec. Reuse internal capabilities without automatically exposing new model-facing tools.
- **`rust/process-discipline`** — Treat this as repository or release policy unless it changes a runtime contract. Do not add runtime machinery for a process-only lesson.
- **`rust/trait-seam`** — Use a small trait at the capability boundary; keep policy and model-facing schemas outside the backend implementation.
- **Status gate** — Rejected design. Do not implement it as written. Reuse its failure analysis, constraints, and reversal condition.

### Smallest safe implementation slice

1. Express the semantic contract as Rust types and one owning boundary.
2. Add the smallest adapter or service needed by the existing loop; do not add a new model-facing tool unless the four-tool surface cannot express the job.
3. Record durable evidence before deriving summaries or UI state.
4. Add one happy-path test, one interruption test, and one replay/restart test where state survives a turn.
5. Keep the patch reversible until the source note's strongest failure case is proven in the Rust harness.

## Search handles

- Tags: `class/feature`, `concern/boundary`, `concern/compatibility`, `concern/discovery-routing`, `concern/evidence`, `concern/lifecycle`, `concern/ownership`, `concern/trust`, `domain/build-release`, `domain/security`, `domain/testing`, `lifecycle/rejected`, `mechanism/policy`, `mechanism/registry`
- Aliases: `PLATFORM_CHAINS.win32`, `node-addon-landlock-run`, `@landstrip/landstrip`, `optionalDependencies`, `--probe`, `Evaluate landstrip before building a Windows sandbox launcher`, `feature`, `boundary`, `compatibility`, `discovery routing`, `evidence`, `lifecycle`, `ownership`, `trust`
- Regex: `(?i)(PLATFORM_CHAINS\.win32|node\-addon\-landlock\-run|@landstrip/landstrip|optionalDependencies|\-\-probe|feature|boundary|compatibility)`

```bash
rg -n --pcre2 "(?i)(PLATFORM_CHAINS\\.win32|node\\-addon\\-landlock\\-run|@landstrip/landstrip|optionalDependencies|\\-\\-probe|feature|boundary|compatibility)" source-deepseek-harness
rg -l --fixed-strings "class/feature" 03-agent-context
```

## Connected notes

- **`source-link`** — [0146. The subprocess sandbox --- confinement seam, native runners, escalation, and per-session modes](0146-the-subprocess-sandbox-confinement-seam-native-runners-escalation-and-pe.md): The source note links to this decision directly.
- **`shares-code-with`** — [0426. In-repository Landlock release](0426-in-repository-landlock-release.md): Shares source implementation: `native/landlock-run/packages/entry`, `native/landlock-run/packages/entry/src/index.ts`.
- **`same-design-pressure`** — [0279. Windows sandbox rung: raw ACL restricted tokens over mxc and AppContainer](0279-windows-sandbox-rung-raw-acl-restricted-tokens-over-mxc-and-appcontainer.md): Shares design concerns: `concern/boundary`, `concern/compatibility`, `concern/discovery-routing`.
- **`shares-code-with`** — [0245. Win32 folder picker moves to koffi in a child process](0245-win32-folder-picker-moves-to-koffi-in-a-child-process.md): Shares source implementation: `native/landlock-run/packages/entry/src/index.ts`.
- **`same-design-pressure`** — [0212. Current sandbox policy context](0212-current-sandbox-policy-context.md): Shares design concerns: `concern/boundary`, `concern/compatibility`, `concern/discovery-routing`.
- **`same-design-pressure`** — [0431. Dual Wine and native Windows pull-request CI](0431-dual-wine-and-native-windows-pull-request-ci.md): Shares design concerns: `concern/boundary`, `concern/compatibility`, `concern/discovery-routing`.
- **`same-design-pressure`** — [0241. Windows defaults to pwsh](0241-windows-defaults-to-pwsh.md): Shares design concerns: `concern/boundary`, `concern/discovery-routing`, `concern/evidence`.
- **`same-design-pressure`** — [0542. Dependency swaps rejected by the 2026-07 NIH audit](0542-dependency-swaps-rejected-by-the-2026-07-nih-audit.md): Shares design concerns: `concern/boundary`, `concern/compatibility`, `concern/discovery-routing`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0532-evaluate-landstrip-before-building-a-windows-sandbox-launcher.md`.
