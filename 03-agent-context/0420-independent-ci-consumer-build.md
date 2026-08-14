---
id: "dsh-note-0420"
title: "Independent CI consumer build"
status: "implemented"
class: "process"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/process/2026-07-30-independent-ci-consumer-build.md"
implementation_evidence: "lead-only"
target_anchor: "repository tests and engineering policy"
tags:
  - "class/process"
  - "concern/boundary"
  - "concern/concurrency"
  - "concern/discovery-routing"
  - "concern/evidence"
  - "concern/lifecycle"
  - "concern/ownership"
  - "concern/schema-types"
  - "domain/build-release"
  - "domain/testing"
  - "lifecycle/implemented"
  - "mechanism/generation"
  - "mechanism/policy"
aliases:
  - "lib/"
  - "node 24 / snapshots and artifacts"
  - "node 24 / static"
  - "Independent CI consumer build"
  - "process"
  - "boundary"
  - "concurrency"
  - "discovery routing"
  - "evidence"
  - "lifecycle"
  - "ownership"
  - "schema types"
  - "build release"
  - "testing"
search_regex: "(?i)(lib/|node[- ]24[- ]/[- ]snapshots[- ]and[- ]artifacts|node[- ]24[- ]/[- ]static|Independent[- ]CI[- ]consumer[- ]build|boundary|concurrency|discovery[- ]routing|evidence)"
---

# 0420. Independent CI consumer build — implementation context

## Open this when

The larger-runner topology gave the static and built-consumer inventories separate jobs, but the static job owned their shared build. It uploaded the emitted tree only after every static gate completed, and the consumer job declared a job-level dependency before restoring that tree. Compiled-output snapshots and publication checks genuinely require a complete build; they do not require runtime-closure checks, documentation generation, module-graph verification, or Knip. That wider dependency made runner availability part of the required critical chain.

## Source decision

The three required Linux jobs enter runner allocation independently. Coverage remains source-only. Static owns source and documentation checks that do not consume emitted output. The consumer job owns the single Linux build together with documentation typechecking, compiled-output snapshots, publication checks, NodeNext checks, and built-bin smokes. The consumer's internal gate graph preserves the real dependency. Build and source-only Node compatibility start first; publint waits for build, built-package invariants validate that publication view, and every compiled-output consumer waits for that validation.

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/process/2026-07-30-independent-ci-consumer-build.md](../02-notes/implemented/process/2026-07-30-independent-ci-consumer-build.md)
- Pinned source: [.agents/notes/implemented/process/2026-07-30-independent-ci-consumer-build.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/process/2026-07-30-independent-ci-consumer-build.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`scripts/build-python-release.py`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/build-python-release.py) | repository automation | Path shares title concepts: build. | `title-path-lead` |
| [`docs/event-producer-consumer.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/event-producer-consumer.md) | package contract and examples | Path shares title concepts: consumer. | `title-path-lead` |
| [`docs/event-producer-consumer.zh.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/event-producer-consumer.zh.md) | package contract and examples | Path shares title concepts: consumer. | `title-path-lead` |
| [`scripts/build-exe-for-python-sdk.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/build-exe-for-python-sdk.ts) | repository automation | Path shares title concepts: build. | `title-path-lead` |
| [`native/landlock-run/scripts/build.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/native/landlock-run/scripts/build.ts) | runtime implementation | Path shares title concepts: build. | `title-path-lead` |
| [`docs/event-producer-consumer.i18n.yaml`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/event-producer-consumer.i18n.yaml) | composition and configuration | Path shares title concepts: consumer. | `title-path-lead` |
| [`.github/workflows/build-exe-for-python-sdk.yml`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.github/workflows/build-exe-for-python-sdk.yml) | repository automation | Path shares title concepts: build. | `title-path-lead` |

### Code constructs named by the note

The note names behavior or policy but no uniquely resolvable code declaration. Search by its aliases and inspect the listed package entry points.

### Tests and executable evidence

- No test file was tied to this note with enough confidence. Read the note's verification section and search the owning package's `tests/` directory.

## How to read the implementation

1. Start with [`scripts/build-python-release.py`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/build-python-release.py) because it has the strongest evidence link to the note.
2. Read public types and entry points before implementation details. Write down the contract in plain language.
3. Follow the named constructs into their callers and tests. Confirm which component can actually enforce the rule.
4. Reconstruct one failure path: cancellation, crash, retry, stale state, partial write, or incompatible provider input—whichever the note is protecting against.
5. Only then map the contract into the current Rust repository. Target paths must come from that repository, never from guesswork in this wiki.

## Rust harness port

**Likely integration point:** repository tests and engineering policy.

Preserve the behavior and ownership rule. Do not mechanically translate Cordis, TypeScript declaration merging, Node streams, or package topology into Rust.

Relevant standing rules:

- **`rust/four-tool-surface`** — Preserve the four model tools: read, write, patch, and exec. Reuse internal capabilities without automatically exposing new model-facing tools.
- **`rust/newtype-ids`** — Represent identity domains with Rust newtypes; do not pass bare strings across session, call, turn, step, and agent boundaries.
- **`rust/process-discipline`** — Treat this as repository or release policy unless it changes a runtime contract. Do not add runtime machinery for a process-only lesson.
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

- Tags: `class/process`, `concern/boundary`, `concern/concurrency`, `concern/discovery-routing`, `concern/evidence`, `concern/lifecycle`, `concern/ownership`, `concern/schema-types`, `domain/build-release`, `domain/testing`, `lifecycle/implemented`, `mechanism/generation`, `mechanism/policy`
- Aliases: `lib/`, `node 24 / snapshots and artifacts`, `node 24 / static`, `Independent CI consumer build`, `process`, `boundary`, `concurrency`, `discovery routing`, `evidence`, `lifecycle`, `ownership`, `schema types`, `build release`, `testing`
- Regex: `(?i)(lib/|node[- ]24[- ]/[- ]snapshots[- ]and[- ]artifacts|node[- ]24[- ]/[- ]static|Independent[- ]CI[- ]consumer[- ]build|boundary|concurrency|discovery[- ]routing|evidence)`

```bash
rg -n --pcre2 "(?i)(lib/|node[- ]24[- ]/[- ]snapshots[- ]and[- ]artifacts|node[- ]24[- ]/[- ]static|Independent[- ]CI[- ]consumer[- ]build|boundary|concurrency|discovery[- ]routing|evidence)" source-deepseek-harness
rg -l --fixed-strings "class/process" 03-agent-context
```

## Connected notes

- **`source-link`** — [0400. Evidence-based larger hosted runners](0400-evidence-based-larger-hosted-runners.md): The source note links to this decision directly.
- **`same-design-pressure`** — [0439. Private npm publication as three independent sequences](0439-private-npm-publication-as-three-independent-sequences.md): Shares design concerns: `concern/boundary`, `concern/concurrency`, `concern/discovery-routing`.
- **`same-design-pressure`** — [0511. Session projections and command lifecycle logging](0511-session-projections-and-command-lifecycle-logging.md): Shares design concerns: `concern/boundary`, `concern/concurrency`, `concern/discovery-routing`.
- **`same-design-pressure`** — [0516. Migrate simple unary API Proxy calls to business Remote services](0516-migrate-simple-unary-api-proxy-calls-to-business-remote-services.md): Shares design concerns: `concern/boundary`, `concern/concurrency`, `concern/discovery-routing`.
- **`same-design-pressure`** — [0518. Recallable compaction --- index checkpoints, a state checkpoint, and in-session history recall](0518-recallable-compaction-index-checkpoints-a-state-checkpoint-and-in-sessio.md): Shares design concerns: `concern/boundary`, `concern/concurrency`, `concern/discovery-routing`.
- **`same-design-pressure`** — [0278. Web background-job display](0278-web-background-job-display.md): Shares design concerns: `concern/boundary`, `concern/concurrency`, `concern/discovery-routing`.
- **`same-design-pressure`** — [0265. Settlement delivery belongs to the continuation manager](0265-settlement-delivery-belongs-to-the-continuation-manager.md): Shares design concerns: `concern/boundary`, `concern/concurrency`, `concern/discovery-routing`.
- **`same-design-pressure`** — [0131. Code Mode --- the model writes TypeScript against the tool registry](0131-code-mode-the-model-writes-typescript-against-the-tool-registry.md): Shares design concerns: `concern/boundary`, `concern/concurrency`, `concern/discovery-routing`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0420-independent-ci-consumer-build.md`.
