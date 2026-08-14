---
id: "dsh-note-0436"
title: "verify-md-links validates fragment anchors, closing the last dead-link class"
status: "implemented"
class: "process"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/process/2026-08-09-md-fragment-anchor-gate.md"
implementation_evidence: "high"
target_anchor: "repository tests and engineering policy"
tags:
  - "class/process"
  - "concern/boundary"
  - "concern/cancellation-timeout"
  - "concern/evidence"
  - "concern/ownership"
  - "concern/trust"
  - "domain/build-release"
  - "domain/extensions"
  - "domain/filesystem"
  - "domain/security"
  - "domain/testing"
  - "domain/ui-interaction"
  - "lifecycle/implemented"
  - "mechanism/policy"
  - "mechanism/registry"
aliases:
  - "target"
  - "anchor"
  - "githubSlug"
  - "markdownHeadingLines"
  - "anchorCache"
  - "verify-md-links"
  - "#security-and-authority-are-explicit-non-goals"
  - "Security and authority are non-goals"
  - "tool-fs"
  - "#deferred-work"
  - "#showcase-web_fetch"
  - "-1"
  - "-2"
  - "file.ts#L10"
search_regex: "(?i)(target|anchor|githubSlug|markdownHeadingLines|anchorCache|verify\\-md\\-links|\\#security\\-and\\-authority\\-are\\-explicit\\-non\\-goals|Security[- ]and[- ]authority[- ]are[- ]non\\-goals)"
---

# 0436. verify-md-links validates fragment anchors, closing the last dead-link class — implementation context

## Open this when

verify-md-links proved a relative link's target file exists but never looked at the #fragment, and the documentation standard compensated with a manual rule: grep anchors yourself before renaming a heading. A corpus sweep found 15 links whose fragments named no anchor in their target --- three distinct decay modes: a heading reworded after the link was written (#security-and-authority-are-explicit-non-goals vs the note's current Security and authority are non-goals), a contract relocated to a different owning document (tool-fs linking the seam README for the no-timeout rule that now lives in the group README).

## Source decision

verify-md-links now resolves fragments too (superseding the deferred scope cut in the cross-link decision). For every relative link whose target is a Markdown file --- same-file #anchor links included, which the old checker skipped entirely --- the fragment must name a real anchor in the target: a heading's GitHub slug or an explicit in real HTML flow (code samples and commented-out anchors register nothing).

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/process/2026-08-09-md-fragment-anchor-gate.md](../02-notes/implemented/process/2026-08-09-md-fragment-anchor-gate.md)
- Pinned source: [.agents/notes/implemented/process/2026-08-09-md-fragment-anchor-gate.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/process/2026-08-09-md-fragment-anchor-gate.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`docs/AGENTS.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/AGENTS.md) | package contract and examples | The source note names this file directly. Contains the exact code literal `dsh-doc-standards` named by the note. | `exact-code-occurrence, named-file` |
| [`docs/glossary.zh.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/glossary.zh.md) | package contract and examples | The source note names this file directly. | `named-file` |
| [`docs/cordis-primer.zh.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/cordis-primer.zh.md) | package contract and examples | The source note names this file directly. | `named-file` |
| [`packages/fs/tool-fs/src/edit.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/tool-fs/src/edit.ts) | runtime implementation | Core file in the package named by the note: `packages/fs/tool-fs`. Defines `target`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/fs/tool-fs/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/tool-fs/src/index.ts) | package entry point | Core file in the package named by the note: `packages/fs/tool-fs`. | `named-package-member` |
| [`packages/fs/tool-fs/src/write.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/tool-fs/src/write.ts) | runtime implementation | Core file in the package named by the note: `packages/fs/tool-fs`. Defines `target`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/fs/tool-fs/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/tool-fs/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/fs/tool-fs`. | `named-package-member` |
| [`packages/fs/tool-fs/src/read-target.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/tool-fs/src/read-target.ts) | runtime implementation | Core file in the package named by the note: `packages/fs/tool-fs`. Defines `target`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/fs/tool-fs`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/packages/fs/tool-fs) | package or module directory | The note names this package or capability. | `named-package` |
| [`scripts/markdown.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/markdown.ts) | repository automation | Defines `markdownHeadingLines`, a construct named by the note. | `symbol-definition` |
| [`scripts/verify-md-links.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/verify-md-links.ts) | repository automation | Defines `anchorCache`, a construct named by the note. | `symbol-definition` |
| [`packages/llm/token-meter/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/token-meter/src/index.ts) | package entry point | Defines `anchor`, a construct named by the note. | `symbol-definition` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `target` | `const` | [`packages/fs/tool-fs/src/edit.ts:117`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/tool-fs/src/edit.ts#L117) | `const target = await ctx.fs.resolve(input.filePath, sessionResolveOptions(exec, input.filePath, sandboxPolicy?.workspaceRoot))` |
| `target` | `const` | [`packages/fs/tool-fs/src/read-target.ts:24`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/tool-fs/src/read-target.ts#L24) | `const target = await ctx.fs.resolve(requestedPath, sessionResolveOptions(exec, requestedPath))` |
| `target` | `const` | [`packages/fs/tool-fs/src/write.ts:108`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/tool-fs/src/write.ts#L108) | `const target = await ctx.fs.resolve(input.filePath, sessionResolveOptions(exec, input.filePath, sandboxPolicy?.workspaceRoot))` |
| `anchor` | `const` | [`packages/llm/token-meter/src/index.ts:121`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/token-meter/src/index.ts#L121) | `const anchor = state.anchor` |
| `githubSlug` | `function` | [`packages/typert/generator/src/cordis-catalog.ts:945`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/typert/generator/src/cordis-catalog.ts#L945) | `function githubSlug(heading: string): string {` |
| `markdownHeadingLines` | `function` | [`scripts/markdown.ts:89`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/markdown.ts#L89) | `export function markdownHeadingLines(source: string): MarkdownHeadingLine[] {` |
| `anchorCache` | `function` | [`scripts/verify-md-links.ts:142`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/verify-md-links.ts#L142) | `export function anchorCache(): (absPath: string) => Set<string> {` |

### Tests and executable evidence

- [`scripts/verify-md-links.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/verify-md-links.spec.ts) — The source note names this file directly.
- [`packages/fs/tool-fs/tests/read-image.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/tool-fs/tests/read-image.spec.ts) — A test under the owning area exercises or imports `tool-fs`.
- Source verification intent: scripts/verify-md-links.spec.ts proves the acceptance paths: rendered-text slugging (backticks, punctuation, a linked heading, kept underscores), occupied-set repeat suffixes, ignored inside fences/inline code/comments, a resolving mixed-link document, dead same-file and cross-file fragments, a case-variant fragment, and a missing target still reported as target rather than anchor. The gate runs over the full corpus in doc-sync (verify-md-links) and passes only after the 15 fixes --- the corpus itself is the red-to-green evidence for each decay mode.

## How to read the implementation

1. Start with [`docs/AGENTS.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/AGENTS.md) because it has the strongest evidence link to the note.
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
- **`rust/quickjs-boundary`** — QuickJS holds orchestration variables; Rust/Tokio owns async execution and exposes only explicitly granted capabilities.
- **`rust/tokio-cancellation`** — Thread one cancellation token through async work, close owned resources before reuse, and test cancellation at every await boundary.
- **`rust/trait-seam`** — Use a small trait at the capability boundary; keep policy and model-facing schemas outside the backend implementation.

### Smallest safe implementation slice

1. Express the semantic contract as Rust types and one owning boundary.
2. Add the smallest adapter or service needed by the existing loop; do not add a new model-facing tool unless the four-tool surface cannot express the job.
3. Record durable evidence before deriving summaries or UI state.
4. Add one happy-path test, one interruption test, and one replay/restart test where state survives a turn.
5. Keep the patch reversible until the source note's strongest failure case is proven in the Rust harness.

## Search handles

- Tags: `class/process`, `concern/boundary`, `concern/cancellation-timeout`, `concern/evidence`, `concern/ownership`, `concern/trust`, `domain/build-release`, `domain/extensions`, `domain/filesystem`, `domain/security`, `domain/testing`, `domain/ui-interaction`, `lifecycle/implemented`, `mechanism/policy`, `mechanism/registry`
- Aliases: `target`, `anchor`, `githubSlug`, `markdownHeadingLines`, `anchorCache`, `verify-md-links`, `#security-and-authority-are-explicit-non-goals`, `Security and authority are non-goals`, `tool-fs`, `#deferred-work`, `#showcase-web_fetch`, `-1`, `-2`, `file.ts#L10`
- Regex: `(?i)(target|anchor|githubSlug|markdownHeadingLines|anchorCache|verify\-md\-links|\#security\-and\-authority\-are\-explicit\-non\-goals|Security[- ]and[- ]authority[- ]are[- ]non\-goals)`

```bash
rg -n --pcre2 "(?i)(target|anchor|githubSlug|markdownHeadingLines|anchorCache|verify\\-md\\-links|\\#security\\-and\\-authority\\-are\\-explicit\\-non\\-goals|Security[- ]and[- ]authority[- ]are[- ]non\\-goals)" source-deepseek-harness
rg -l --fixed-strings "class/process" 03-agent-context
```

## Connected notes

- **`source-link`** — [0381. Markdown cross-link validity linting](0381-markdown-cross-link-validity-linting.md): The source note links to this decision directly.
- **`shares-code-with`** — [0019. Make `dsh-fs-observation-policy` an event-gate plugin, not a method interface](0019-make-dsh-fs-observation-policy-an-event-gate-plugin-not-a-method-interfa.md): Shares source implementation: `packages/fs/tool-fs/src/edit.ts`, `packages/fs/tool-fs/src/index.ts`.
- **`shares-code-with`** — [0528. Prune dead public API and result fields](0528-prune-dead-public-api-and-result-fields.md): Shares source implementation: `packages/fs/tool-fs/src/edit.ts`, `packages/fs/tool-fs/src/index.ts`.
- **`shares-code-with`** — [0155. Cross-family file sandbox --- one policy home, a sandboxed fs provider, and fs escalation parity](0155-cross-family-file-sandbox-one-policy-home-a-sandboxed-fs-provider-and-fs.md): Shares source implementation: `packages/fs/tool-fs/src/edit.ts`, `packages/fs/tool-fs/src/index.ts`.
- **`shares-code-with`** — [0451. Split the filesystem seam --- provider text mutations plus the `dsh-fs-observation-policy` plugin](0451-split-the-filesystem-seam-provider-text-mutations-plus-the-dsh-fs-observ.md): Shares source implementation: `packages/fs/tool-fs/src/index.ts`, `packages/fs/tool-fs/src/invariant.ts`.
- **`shares-code-with`** — [0321. Bound overwrite contextual-diff bases at the provider](0321-bound-overwrite-contextual-diff-bases-at-the-provider.md): Shares source implementation: `packages/fs/tool-fs`, `packages/fs/tool-fs/src/index.ts`.
- **`shares-code-with`** — [0284. A minimal read_image tool over existing seams](0284-a-minimal-read-image-tool-over-existing-seams.md): Shares source implementation: `packages/fs/tool-fs`, `packages/fs/tool-fs/src/index.ts`.
- **`shares-code-with`** — [0548. Result-time applied-hunk diffs for file mutations](0548-result-time-applied-hunk-diffs-for-file-mutations.md): Shares source implementation: `packages/fs/tool-fs`, `packages/fs/tool-fs/src/index.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0436-verify-md-links-validates-fragment-anchors-closing-the-last-dead-link-cl.md`.
