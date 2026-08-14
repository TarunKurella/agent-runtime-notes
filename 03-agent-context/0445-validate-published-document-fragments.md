---
id: "dsh-note-0445"
title: "Validate published document fragments"
status: "implemented"
class: "process"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/process/2026-08-13-published-document-fragments.md"
implementation_evidence: "lead-only"
target_anchor: "repository tests and engineering policy"
tags:
  - "class/process"
  - "concern/discovery-routing"
  - "concern/evidence"
  - "concern/schema-types"
  - "domain/build-release"
  - "domain/configuration"
  - "domain/storage"
  - "lifecycle/implemented"
aliases:
  - "verify-md-links"
  - "verify-doc-site-fragments"
  - "website/.dist"
  - ".html"
  - "Validate published document fragments"
  - "process"
  - "discovery routing"
  - "evidence"
  - "schema types"
  - "build release"
  - "configuration"
  - "storage"
  - "implemented"
search_regex: "(?i)(verify\\-md\\-links|verify\\-doc\\-site\\-fragments|website/\\.dist|\\.html|Validate[- ]published[- ]document[- ]fragments|discovery[- ]routing|evidence|schema[- ]types)"
---

# 0445. Validate published document fragments — implementation context

## Open this when

verify-md-links validates fragments with GitHub's Markdown heading ids, while the documentation website renders headings with VitePress. Punctuation-heavy headings and translated headings can therefore pass source validation but produce links to ids absent from the published HTML. A successful VitePress build validates target pages, not fragment ids.

## Source decision

docs:build and its MPA variant run verify-doc-site-fragments after VitePress emits website/.dist. The verifier parses every emitted HTML page, resolves each internal fragment link against VitePress clean URLs, and fails when the output is absent, routes are ambiguous, an href is malformed, or either the target page or requested id is missing. Unit tests cover those failures plus clean URLs, .html aliases, same-page links, encoded and literal ids, and external-link exclusion. Any fragment target heading whose GitHub id differs from its VitePress id carries an explicit GitHub-compatible alias.

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/process/2026-08-13-published-document-fragments.md](../02-notes/implemented/process/2026-08-13-published-document-fragments.md)
- Pinned source: [.agents/notes/implemented/process/2026-08-13-published-document-fragments.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/process/2026-08-13-published-document-fragments.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

No implementation path could be confirmed from the pinned tree. Use the search handles below; do not invent a source location.

### Code constructs named by the note

The note names behavior or policy but no uniquely resolvable code declaration. Search by its aliases and inspect the listed package entry points.

### Tests and executable evidence

- [`scripts/verify-md-links.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/verify-md-links.spec.ts) — A test under the owning area exercises or imports `verify-md-links`.
- [`scripts/verify-doc-site-fragments.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/verify-doc-site-fragments.spec.ts) — A test under the owning area exercises or imports `verify-doc-site-fragments`.

## How to read the implementation

1. Start from the source note and run the regex below across the pinned repository.
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

### Smallest safe implementation slice

1. Express the semantic contract as Rust types and one owning boundary.
2. Add the smallest adapter or service needed by the existing loop; do not add a new model-facing tool unless the four-tool surface cannot express the job.
3. Record durable evidence before deriving summaries or UI state.
4. Add one happy-path test, one interruption test, and one replay/restart test where state survives a turn.
5. Keep the patch reversible until the source note's strongest failure case is proven in the Rust harness.

## Search handles

- Tags: `class/process`, `concern/discovery-routing`, `concern/evidence`, `concern/schema-types`, `domain/build-release`, `domain/configuration`, `domain/storage`, `lifecycle/implemented`
- Aliases: `verify-md-links`, `verify-doc-site-fragments`, `website/.dist`, `.html`, `Validate published document fragments`, `process`, `discovery routing`, `evidence`, `schema types`, `build release`, `configuration`, `storage`, `implemented`
- Regex: `(?i)(verify\-md\-links|verify\-doc\-site\-fragments|website/\.dist|\.html|Validate[- ]published[- ]document[- ]fragments|discovery[- ]routing|evidence|schema[- ]types)`

```bash
rg -n --pcre2 "(?i)(verify\\-md\\-links|verify\\-doc\\-site\\-fragments|website/\\.dist|\\.html|Validate[- ]published[- ]document[- ]fragments|discovery[- ]routing|evidence|schema[- ]types)" source-deepseek-harness
rg -l --fixed-strings "class/process" 03-agent-context
```

## Connected notes

- **`shares-code-with`** — [0425. The documentation site carries its own images](0425-the-documentation-site-carries-its-own-images.md): Shares source implementation: `scripts/verify-doc-site-fragments.spec.ts`, `scripts/verify-md-links.spec.ts`.
- **`shares-code-with`** — [0648. Forward-only PR-to-Issue status projection](0648-forward-only-pr-to-issue-status-projection.md): Shares source implementation: `scripts/verify-doc-site-fragments.spec.ts`.
- **`shares-code-with`** — [0436. verify-md-links validates fragment anchors, closing the last dead-link class](0436-verify-md-links-validates-fragment-anchors-closing-the-last-dead-link-cl.md): Shares source implementation: `scripts/verify-md-links.spec.ts`.
- **`shares-code-with`** — [0374. Unlink stale profile fallback links instead of rmSync](0374-unlink-stale-profile-fallback-links-instead-of-rmsync.md): Shares source implementation: `scripts/verify-md-links.spec.ts`.
- **`shares-code-with`** — [0627. TUI long-session render costs --- shared step-timing scan and card line caches](0627-tui-long-session-render-costs-shared-step-timing-scan-and-card-line-cach.md): Shares source implementation: `scripts/verify-md-links.spec.ts`.
- **`same-design-pressure`** — [0346. Validate API key format before it reaches an HTTP header](0346-validate-api-key-format-before-it-reaches-an-http-header.md): Shares design concerns: `concern/discovery-routing`, `concern/evidence`, `concern/schema-types`.
- **`same-design-pressure`** — [0458. Plan-specific collaboration state](0458-plan-specific-collaboration-state.md): Shares design concerns: `concern/discovery-routing`, `concern/evidence`, `concern/schema-types`.
- **`same-design-pressure`** — [0431. Dual Wine and native Windows pull-request CI](0431-dual-wine-and-native-windows-pull-request-ci.md): Shares design concerns: `concern/discovery-routing`, `concern/evidence`, `concern/schema-types`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0445-validate-published-document-fragments.md`.
