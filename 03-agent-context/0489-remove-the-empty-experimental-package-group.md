---
id: "dsh-note-0489"
title: "Remove the empty experimental package group"
status: "implemented"
class: "simplification"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/simplification/2026-08-11-remove-empty-experimental-package-group.md"
implementation_evidence: "lead-only"
target_anchor: "repository tests and release policy"
tags:
  - "class/simplification"
  - "concern/boundary"
  - "concern/discovery-routing"
  - "concern/lifecycle"
  - "concern/ownership"
  - "concern/simplification"
  - "domain/build-release"
  - "domain/configuration"
  - "domain/extensions"
  - "domain/storage"
  - "lifecycle/implemented"
  - "mechanism/policy"
aliases:
  - "packages/experimental/"
  - "Remove the empty experimental package group"
  - "simplification"
  - "boundary"
  - "discovery routing"
  - "lifecycle"
  - "ownership"
  - "build release"
  - "configuration"
  - "extensions"
  - "storage"
  - "implemented"
  - "policy"
search_regex: "(?i)(packages/experimental/|Remove[- ]the[- ]empty[- ]experimental[- ]package[- ]group|simplification|boundary|discovery[- ]routing|lifecycle|ownership|build[- ]release)"
---

# 0489. Remove the empty experimental package group — implementation context

## Open this when

The package hierarchy reserves packages/experimental/ for prototypes and internal-only plugins, but no package has used the group. The empty group adds placement, dependency, promotion, and release rules without a current package or release mechanism that needs them. The original group aimed to let the team share prototypes against the real plugin graph without implying product support. That need remains possible, but it does not justify a permanent repository category before a concrete package exists.

## Source decision

The package hierarchy has no reserved experimental or internal-only group. Packages continue to live in groups selected for their current product role. A concrete package that needs different release, stability, or dependency treatment requires a decision based on its actual consumers and release mechanism. That decision may reintroduce a dedicated group when it can also define and enforce the exclusion rules. This note consolidates and supersedes the experimental-package-group decision, whose active triplet is removed with the empty directory.

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/simplification/2026-08-11-remove-empty-experimental-package-group.md](../02-notes/implemented/simplification/2026-08-11-remove-empty-experimental-package-group.md)
- Pinned source: [.agents/notes/implemented/simplification/2026-08-11-remove-empty-experimental-package-group.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/simplification/2026-08-11-remove-empty-experimental-package-group.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`vendor/group/package.json`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/group/package.json) | composition and configuration | Path shares title concepts: group, package. | `title-path-lead` |
| [`package.json`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/package.json) | composition and configuration | Path shares title concepts: package. | `title-path-lead` |
| [`website/package.json`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/website/package.json) | composition and configuration | Path shares title concepts: package. | `title-path-lead` |
| [`examples/package.json`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/examples/package.json) | composition and configuration | Path shares title concepts: package. | `title-path-lead` |
| [`apps/web/package.json`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/package.json) | composition and configuration | Path shares title concepts: package. | `title-path-lead` |
| [`apps/cli/package.json`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/cli/package.json) | composition and configuration | Path shares title concepts: package. | `title-path-lead` |
| [`vendor/group/README.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/group/README.md) | package contract and examples | Path shares title concepts: group. | `title-path-lead` |
| [`vendor/hmr/package.json`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/hmr/package.json) | composition and configuration | Path shares title concepts: package. | `title-path-lead` |
| [`scripts/package-graph.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/package-graph.ts) | repository automation | Path shares title concepts: package. | `title-path-lead` |
| [`vendor/timer/package.json`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/timer/package.json) | composition and configuration | Path shares title concepts: package. | `title-path-lead` |
| [`vendor/group/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/group/src/index.ts) | package entry point | Path shares title concepts: group. | `title-path-lead` |
| [`vendor/cordis/package.json`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/cordis/package.json) | composition and configuration | Path shares title concepts: package. | `title-path-lead` |

### Code constructs named by the note

The note names behavior or policy but no uniquely resolvable code declaration. Search by its aliases and inspect the listed package entry points.

### Tests and executable evidence

- [`scripts/package-invariants.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/package-invariants.spec.ts) — Path shares title concepts: package.
- [`scripts/verify-dsh-package-licenses.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/verify-dsh-package-licenses.spec.ts) — Path shares title concepts: package.
- [`scripts/verify-built-package-invariants.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/verify-built-package-invariants.spec.ts) — Path shares title concepts: package.
- [`apps/web/tests/snapshots/models-settings/empty.expected.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/snapshots/models-settings/empty.expected.md) — Path shares title concepts: empty.
- [`packages/typert/generator/tests/fixtures/type-model/package.json`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/typert/generator/tests/fixtures/type-model/package.json) — Path shares title concepts: package.
- [`packages/typert/generator/tests/fixtures/remote-model/package.json`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/typert/generator/tests/fixtures/remote-model/package.json) — Path shares title concepts: package.
- [`examples/acp-agent/tests/snapshots/empty-response-retry/input.json`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/examples/acp-agent/tests/snapshots/empty-response-retry/input.json) — Path shares title concepts: empty.
- [`examples/acp-agent/tests/snapshots/empty-response-retry/session.jsonl`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/examples/acp-agent/tests/snapshots/empty-response-retry/session.jsonl) — Path shares title concepts: empty.

## How to read the implementation

1. Start with [`vendor/group/package.json`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/group/package.json) because it has the strongest evidence link to the note.
2. Read public types and entry points before implementation details. Write down the contract in plain language.
3. Follow the named constructs into their callers and tests. Confirm which component can actually enforce the rule.
4. Reconstruct one failure path: cancellation, crash, retry, stale state, partial write, or incompatible provider input—whichever the note is protecting against.
5. Only then map the contract into the current Rust repository. Target paths must come from that repository, never from guesswork in this wiki.

## Rust harness port

**Likely integration point:** repository tests and release policy.

Preserve the behavior and ownership rule. Do not mechanically translate Cordis, TypeScript declaration merging, Node streams, or package topology into Rust.

Relevant standing rules:

- **`rust/append-log`** — Keep durable facts append-only and derive views from them. Separate the synchronous fact boundary from asynchronous persistence.
- **`rust/four-tool-surface`** — Preserve the four model tools: read, write, patch, and exec. Reuse internal capabilities without automatically exposing new model-facing tools.
- **`rust/process-discipline`** — Treat this as repository or release policy unless it changes a runtime contract. Do not add runtime machinery for a process-only lesson.
- **`rust/quickjs-boundary`** — QuickJS holds orchestration variables; Rust/Tokio owns async execution and exposes only explicitly granted capabilities.
- **`rust/trait-seam`** — Use a small trait at the capability boundary; keep policy and model-facing schemas outside the backend implementation.

### Smallest safe implementation slice

1. Express the semantic contract as Rust types and one owning boundary.
2. Add the smallest adapter or service needed by the existing loop; do not add a new model-facing tool unless the four-tool surface cannot express the job.
3. Record durable evidence before deriving summaries or UI state.
4. Add one happy-path test, one interruption test, and one replay/restart test where state survives a turn.
5. Keep the patch reversible until the source note's strongest failure case is proven in the Rust harness.

## Search handles

- Tags: `class/simplification`, `concern/boundary`, `concern/discovery-routing`, `concern/lifecycle`, `concern/ownership`, `concern/simplification`, `domain/build-release`, `domain/configuration`, `domain/extensions`, `domain/storage`, `lifecycle/implemented`, `mechanism/policy`
- Aliases: `packages/experimental/`, `Remove the empty experimental package group`, `simplification`, `boundary`, `discovery routing`, `lifecycle`, `ownership`, `build release`, `configuration`, `extensions`, `storage`, `implemented`, `policy`
- Regex: `(?i)(packages/experimental/|Remove[- ]the[- ]empty[- ]experimental[- ]package[- ]group|simplification|boundary|discovery[- ]routing|lifecycle|ownership|build[- ]release)`

```bash
rg -n --pcre2 "(?i)(packages/experimental/|Remove[- ]the[- ]empty[- ]experimental[- ]package[- ]group|simplification|boundary|discovery[- ]routing|lifecycle|ownership|build[- ]release)" source-deepseek-harness
rg -l --fixed-strings "class/simplification" 03-agent-context
```

## Connected notes

- **`same-design-pressure`** — [0046. Meaningful package invariant contracts](0046-meaningful-package-invariant-contracts.md): Shares design concerns: `concern/boundary`, `concern/discovery-routing`, `concern/lifecycle`.
- **`same-design-pressure`** — [0392. Package Model Experience contract](0392-package-model-experience-contract.md): Shares design concerns: `concern/boundary`, `concern/discovery-routing`, `concern/lifecycle`.
- **`same-design-pressure`** — [0500. Keyless browser e2e lane for the web GUI](0500-keyless-browser-e2e-lane-for-the-web-gui.md): Shares design concerns: `concern/boundary`, `concern/discovery-routing`, `concern/lifecycle`.
- **`same-design-pressure`** — [0512. Storage root placement and derived-medium recovery](0512-storage-root-placement-and-derived-medium-recovery.md): Shares design concerns: `concern/boundary`, `concern/discovery-routing`, `concern/lifecycle`.
- **`same-design-pressure`** — [0514. Cordis Host/Client Dynamic Plugin Runtime](0514-cordis-host-client-dynamic-plugin-runtime.md): Shares design concerns: `concern/boundary`, `concern/discovery-routing`, `concern/lifecycle`.
- **`same-design-pressure`** — [0516. Migrate simple unary API Proxy calls to business Remote services](0516-migrate-simple-unary-api-proxy-calls-to-business-remote-services.md): Shares design concerns: `concern/boundary`, `concern/discovery-routing`, `concern/lifecycle`.
- **`same-design-pressure`** — [0528. Prune dead public API and result fields](0528-prune-dead-public-api-and-result-fields.md): Shares design concerns: `concern/boundary`, `concern/discovery-routing`, `concern/lifecycle`.
- **`same-design-pressure`** — [0542. Dependency swaps rejected by the 2026-07 NIH audit](0542-dependency-swaps-rejected-by-the-2026-07-nih-audit.md): Shares design concerns: `concern/boundary`, `concern/discovery-routing`, `concern/lifecycle`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0489-remove-the-empty-experimental-package-group.md`.
