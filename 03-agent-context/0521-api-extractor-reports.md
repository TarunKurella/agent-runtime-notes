---
id: "dsh-note-0521"
title: "API extractor reports"
status: "proposed"
class: "process"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/proposed/process/2026-06-11-api-extractor-reports.md"
implementation_evidence: "lead-only"
target_anchor: "repository tests and engineering policy"
tags:
  - "class/process"
  - "domain/build-release"
  - "domain/testing"
  - "lifecycle/proposed"
aliases:
  - "tsc --emitDeclarationOnly"
  - "etc/<pkg>.api.md"
  - "API extractor reports"
  - "process"
  - "build release"
  - "testing"
  - "proposed"
search_regex: "(?i)(tsc[- ]\\-\\-emitDeclarationOnly|etc/<pkg>\\.api\\.md|API[- ]extractor[- ]reports|build[- ]release|testing|proposed|api\\-gateway|api\\-gateway\\.zh)"
---

# 0521. API extractor reports — implementation context

## Open this when

Public API changes are invisible --- nothing makes "this commit changed the public API" an explicit, reviewable fact. A reviewer reading a diff can miss that an exported type gained a field or a method signature shifted.

## Source decision

api-extractor (or tsc --emitDeclarationOnly + a normalized public-API dump) producing a checked-in etc/.api.md per package; CI fails if regeneration differs. Every public-API change becomes a diff line a reviewer (or review agent) must see.

## Decision status

Proposal, not proof of shipped behavior. Reuse the pressure and acceptance criteria; verify whether any candidate code is partial scaffolding before porting it.

- Raw note: [proposed/process/2026-06-11-api-extractor-reports.md](../02-notes/proposed/process/2026-06-11-api-extractor-reports.md)
- Pinned source: [.agents/notes/proposed/process/2026-06-11-api-extractor-reports.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/proposed/process/2026-06-11-api-extractor-reports.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`docs/api-gateway.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/api-gateway.md) | package contract and examples | Path shares title concepts: api. | `title-path-lead` |
| [`docs/api-gateway.zh.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/api-gateway.zh.md) | package contract and examples | Path shares title concepts: api. | `title-path-lead` |
| [`packages/api/README.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/api/README.md) | package contract and examples | Path shares title concepts: api. | `title-path-lead` |
| [`docs/cordis-api/fiber.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/cordis-api/fiber.md) | package contract and examples | Path shares title concepts: api. | `title-path-lead` |
| [`scripts/gen-cordis-api.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/gen-cordis-api.ts) | repository automation | Path shares title concepts: api. | `title-path-lead` |
| [`docs/cordis-api/events.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/cordis-api/events.md) | package contract and examples | Path shares title concepts: api. | `title-path-lead` |
| [`packages/api/README.zh.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/api/README.zh.md) | package contract and examples | Path shares title concepts: api. | `title-path-lead` |
| [`scripts/cordis-core-api.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/cordis-core-api.ts) | repository automation | Path shares title concepts: api. | `title-path-lead` |
| [`docs/api-gateway.i18n.yaml`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/api-gateway.i18n.yaml) | composition and configuration | Path shares title concepts: api. | `title-path-lead` |
| [`docs/cordis-api/service.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/cordis-api/service.md) | package contract and examples | Path shares title concepts: api. | `title-path-lead` |
| [`docs/cordis-api/context.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/cordis-api/context.md) | package contract and examples | Path shares title concepts: api. | `title-path-lead` |
| [`docs/cordis-api/fiber.zh.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/cordis-api/fiber.zh.md) | package contract and examples | Path shares title concepts: api. | `title-path-lead` |

### Code constructs named by the note

The note names behavior or policy but no uniquely resolvable code declaration. Search by its aliases and inspect the listed package entry points.

### Tests and executable evidence

- [`scripts/cordis-core-api.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/cordis-core-api.spec.ts) — Path shares title concepts: api.
- [`packages/llm/llm/tests/api-key.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/tests/api-key.spec.ts) — Path shares title concepts: api.
- [`packages/api/remotes/tests/built-lib.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/api/remotes/tests/built-lib.e2e.ts) — Path shares title concepts: api.
- [`packages/api/remotes/tests/agent-lookup.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/api/remotes/tests/agent-lookup.spec.ts) — Path shares title concepts: api.
- [`packages/api/gateway/tests/gateway.host.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/api/gateway/tests/gateway.host.spec.ts) — Path shares title concepts: api.
- [`packages/client/runtime/tests/fake-api.client.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/runtime/tests/fake-api.client.ts) — Path shares title concepts: api.
- [`packages/api/gateway/tests/gateway.client.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/api/gateway/tests/gateway.client.spec.ts) — Path shares title concepts: api.
- [`packages/client/connection/tests/fake-api.client.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/connection/tests/fake-api.client.ts) — Path shares title concepts: api.
- Source verification intent: Every package has a checked-in etc/.api.md; CI fails when regeneration differs from the committed report. A public-API change (a new export, a widened field, a shifted signature) is visible as a report diff line in review.

## How to read the implementation

1. Start with [`docs/api-gateway.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/api-gateway.md) because it has the strongest evidence link to the note.
2. Read public types and entry points before implementation details. Write down the contract in plain language.
3. Follow the named constructs into their callers and tests. Confirm which component can actually enforce the rule.
4. Reconstruct one failure path: cancellation, crash, retry, stale state, partial write, or incompatible provider input—whichever the note is protecting against.
5. Only then map the contract into the current Rust repository. Target paths must come from that repository, never from guesswork in this wiki.

## Rust harness port

**Likely integration point:** repository tests and engineering policy.

Preserve the behavior and ownership rule. Do not mechanically translate Cordis, TypeScript declaration merging, Node streams, or package topology into Rust.

Relevant standing rules:

- **`rust/four-tool-surface`** — Preserve the four model tools: read, write, patch, and exec. Reuse internal capabilities without automatically exposing new model-facing tools.
- **`rust/process-discipline`** — Treat this as repository or release policy unless it changes a runtime contract. Do not add runtime machinery for a process-only lesson.
- **Status gate** — Proposal, not proof of shipped behavior. Reuse the pressure and acceptance criteria; verify whether any candidate code is partial scaffolding before porting it.

### Smallest safe implementation slice

1. Express the semantic contract as Rust types and one owning boundary.
2. Add the smallest adapter or service needed by the existing loop; do not add a new model-facing tool unless the four-tool surface cannot express the job.
3. Record durable evidence before deriving summaries or UI state.
4. Add one happy-path test, one interruption test, and one replay/restart test where state survives a turn.
5. Keep the patch reversible until the source note's strongest failure case is proven in the Rust harness.

## Search handles

- Tags: `class/process`, `domain/build-release`, `domain/testing`, `lifecycle/proposed`
- Aliases: `tsc --emitDeclarationOnly`, `etc/<pkg>.api.md`, `API extractor reports`, `process`, `build release`, `testing`, `proposed`
- Regex: `(?i)(tsc[- ]\-\-emitDeclarationOnly|etc/<pkg>\.api\.md|API[- ]extractor[- ]reports|build[- ]release|testing|proposed|api\-gateway|api\-gateway\.zh)`

```bash
rg -n --pcre2 "(?i)(tsc[- ]\\-\\-emitDeclarationOnly|etc/<pkg>\\.api\\.md|API[- ]extractor[- ]reports|build[- ]release|testing|proposed|api\\-gateway|api\\-gateway\\.zh)" source-deepseek-harness
rg -l --fixed-strings "class/process" 03-agent-context
```

## Connected notes

- **`source-link`** — [0630. Doc-sync enforcement](0630-doc-sync-enforcement.md): The source note links to this decision directly.
- **`same-design-pressure`** — [0516. Migrate simple unary API Proxy calls to business Remote services](0516-migrate-simple-unary-api-proxy-calls-to-business-remote-services.md): Shares design concerns: `domain/build-release`, `domain/testing`.
- **`same-design-pressure`** — [0528. Prune dead public API and result fields](0528-prune-dead-public-api-and-result-fields.md): Shares design concerns: `domain/build-release`, `domain/testing`.
- **`same-design-pressure`** — [0539. Prune unused skill registry API](0539-prune-unused-skill-registry-api.md): Shares design concerns: `domain/build-release`, `domain/testing`.
- **`same-design-pressure`** — [0496. Real-API e2e in CI against the external DeepSeek API](0496-real-api-e2e-in-ci-against-the-external-deepseek-api.md): Shares design concerns: `domain/build-release`, `domain/testing`.
- **`same-design-pressure`** — [0346. Validate API key format before it reaches an HTTP header](0346-validate-api-key-format-before-it-reaches-an-http-header.md): Shares design concerns: `domain/build-release`, `domain/testing`.
- **`same-design-pressure`** — [0427. Ordered Build for API Remotes Generated Contracts](0427-ordered-build-for-api-remotes-generated-contracts.md): Shares design concerns: `domain/build-release`.
- **`same-design-pressure`** — [0639. Generate the Cordis core API reference](0639-generate-the-cordis-core-api-reference.md): Shares design concerns: `domain/build-release`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0521-api-extractor-reports.md`.
