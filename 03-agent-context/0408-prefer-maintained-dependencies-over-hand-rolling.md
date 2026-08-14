---
id: "dsh-note-0408"
title: "Prefer maintained dependencies over hand-rolling"
status: "implemented"
class: "process"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/process/2026-07-26-dependencies-over-hand-rolling.md"
implementation_evidence: "medium"
target_anchor: "repository tests and engineering policy"
tags:
  - "class/process"
  - "concern/boundary"
  - "concern/evidence"
  - "concern/lifecycle"
  - "concern/ownership"
  - "concern/recovery"
  - "concern/schema-types"
  - "domain/build-release"
  - "domain/configuration"
  - "domain/extensions"
  - "domain/llm"
  - "domain/protocols"
  - "domain/testing"
  - "lifecycle/implemented"
  - "mechanism/policy"
  - "mechanism/projection"
  - "mechanism/streaming"
aliases:
  - "AGENTS.md"
  - "@earendil-works"
  - "packages/util/"
  - "proposed/simplification"
  - "Prefer maintained dependencies over hand-rolling"
  - "process"
  - "boundary"
  - "evidence"
  - "lifecycle"
  - "ownership"
  - "recovery"
  - "schema types"
  - "build release"
  - "configuration"
search_regex: "(?i)(AGENTS\\.md|@earendil\\-works|packages/util/|proposed/simplification|Prefer[- ]maintained[- ]dependencies[- ]over[- ]hand\\-rolling|boundary|evidence|lifecycle)"
---

# 0408. Prefer maintained dependencies over hand-rolling — implementation context

## Open this when

The harness hand-rolls a lot of infrastructure that mature external packages already provide. Some of that is deliberate --- vendored Cordis (vendoring decision), the twin LLM adapters, schemastery as the config-schema standard --- but much of it accreted from an unstated "avoid new dependencies" reflex: the repo-wide external dependency list stayed tiny while packages grew their own SSE parsers, protocol framers, retry loops, and glob matchers.

## Source decision

Introducing an external dependency is a legitimate simplification, not a policy exception. When a well-maintained package (or a Node builtin at our engine floor) covers a hand-rolled surface, replacing the hand-rolled code is the preferred direction, subject to the same evidence standard as any other simplification: the swap must genuinely shrink what we own --- code, tests, and contract surface --- rather than merely relocate complexity behind a wrapper. The bar for a new dependency: Net deletion. The dependency replaces real owned code (implementation + dedicated tests + docs), not hypothetical future code.

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/process/2026-07-26-dependencies-over-hand-rolling.md](../02-notes/implemented/process/2026-07-26-dependencies-over-hand-rolling.md)
- Pinned source: [.agents/notes/implemented/process/2026-07-26-dependencies-over-hand-rolling.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/process/2026-07-26-dependencies-over-hand-rolling.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`.agents/skills/dsh-find-simplifications/SKILL.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/skills/dsh-find-simplifications/SKILL.md) | package contract and examples | The source note names this file directly. | `named-file` |
| [`packages/util/README.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/util/README.md) | package contract and examples | Entry point or contract under the directory named by the note: `packages/util`. | `named-directory-member` |
| [`packages/util`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/packages/util) | package or module directory | The source note names this implementation area directly. | `named-directory` |
| [`tsconfig.base.json`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/tsconfig.base.json) | composition and configuration | Contains the exact code literal `packages/util/` named by the note. | `exact-code-occurrence` |

### Code constructs named by the note

The note names behavior or policy but no uniquely resolvable code declaration. Search by its aliases and inspect the listed package entry points.

### Tests and executable evidence

- No test file was tied to this note with enough confidence. Read the note's verification section and search the owning package's `tests/` directory.

## How to read the implementation

1. Start with [`.agents/skills/dsh-find-simplifications/SKILL.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/skills/dsh-find-simplifications/SKILL.md) because it has the strongest evidence link to the note.
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
- **`rust/provider-adapter`** — Keep provider wire quirks inside adapters; the loop and durable event vocabulary remain provider-neutral.
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

- Tags: `class/process`, `concern/boundary`, `concern/evidence`, `concern/lifecycle`, `concern/ownership`, `concern/recovery`, `concern/schema-types`, `domain/build-release`, `domain/configuration`, `domain/extensions`, `domain/llm`, `domain/protocols`, `domain/testing`, `lifecycle/implemented`, `mechanism/policy`, `mechanism/projection`, `mechanism/streaming`
- Aliases: `AGENTS.md`, `@earendil-works`, `packages/util/`, `proposed/simplification`, `Prefer maintained dependencies over hand-rolling`, `process`, `boundary`, `evidence`, `lifecycle`, `ownership`, `recovery`, `schema types`, `build release`, `configuration`
- Regex: `(?i)(AGENTS\.md|@earendil\-works|packages/util/|proposed/simplification|Prefer[- ]maintained[- ]dependencies[- ]over[- ]hand\-rolling|boundary|evidence|lifecycle)`

```bash
rg -n --pcre2 "(?i)(AGENTS\\.md|@earendil\\-works|packages/util/|proposed/simplification|Prefer[- ]maintained[- ]dependencies[- ]over[- ]hand\\-rolling|boundary|evidence|lifecycle)" source-deepseek-harness
rg -l --fixed-strings "class/process" 03-agent-context
```

## Connected notes

- **`source-link`** — [0008. Two LLM adapters as a design-verification twin](0008-two-llm-adapters-as-a-design-verification-twin.md): The source note links to this decision directly.
- **`source-link`** — [0378. Vendor Cordis as source, not npm dependencies](0378-vendor-cordis-as-source-not-npm-dependencies.md): The source note links to this decision directly.
- **`source-link`** — [0523. Supply chain checks and vendor drift verification](0523-supply-chain-checks-and-vendor-drift-verification.md): The source note links to this decision directly.
- **`shares-code-with`** — [0029. Tool result retention library](0029-tool-result-retention-library.md): Shares source implementation: `packages/util`, `packages/util/README.md`.
- **`same-design-pressure`** — [0144. The approval seam --- one-shot permission decisions over a waterfall of answerers](0144-the-approval-seam-one-shot-permission-decisions-over-a-waterfall-of-answ.md): Shares design concerns: `concern/boundary`, `concern/evidence`, `concern/lifecycle`.
- **`same-design-pressure`** — [0018. Web capability seam - stable tools over multiple providers](0018-web-capability-seam-stable-tools-over-multiple-providers.md): Shares design concerns: `concern/boundary`, `concern/lifecycle`, `concern/ownership`.
- **`same-design-pressure`** — [0431. Dual Wine and native Windows pull-request CI](0431-dual-wine-and-native-windows-pull-request-ci.md): Shares design concerns: `concern/boundary`, `concern/evidence`, `concern/lifecycle`.
- **`same-design-pressure`** — [0495. ACP snapshot tests --- record-once / replay-deterministic](0495-acp-snapshot-tests-record-once-replay-deterministic.md): Shares design concerns: `concern/boundary`, `concern/evidence`, `concern/lifecycle`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0408-prefer-maintained-dependencies-over-hand-rolling.md`.
