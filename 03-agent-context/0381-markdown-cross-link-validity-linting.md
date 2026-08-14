---
id: "dsh-note-0381"
title: "Markdown cross-link validity linting"
status: "implemented"
class: "process"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/process/2026-06-18-markdown-cross-link-lint.md"
implementation_evidence: "high"
target_anchor: "repository tests and engineering policy"
tags:
  - "class/process"
  - "concern/discovery-routing"
  - "concern/evidence"
  - "concern/ownership"
  - "domain/build-release"
  - "domain/protocols"
  - "lifecycle/implemented"
  - "mechanism/policy"
aliases:
  - "image"
  - "link"
  - "definition"
  - "docs/adr/"
  - ".agents/notes/"
  - "proposed/"
  - "implemented/"
  - "rejected/"
  - "doc-sync"
  - "verify-md-links"
  - "scripts/verify-md-links.ts"
  - "verify-md-wrap"
  - "mdast-util-from-markdown"
  - "//host"
search_regex: "(?i)(image|link|definition|docs/adr/|\\.agents/notes/|proposed/|implemented/|rejected/)"
---

# 0381. Markdown cross-link validity linting — implementation context

## Open this when

Docs in this repo link to each other by relative path --- topic, the cookbook, architecture.md. Nothing verified those targets exist. A rename or a move silently breaks every inbound link, and the break is invisible until a reader clicks it. Doc-sync enforcement already mechanized two classes of doc drift (uncompilable code blocks, a stale event-taxonomy table) and verify-md-wrap a third (hard-wrapped prose) --- but a dead cross-link is a fourth, equally mechanical class that was still verified by eyeball. The motivating case is the Agent Note tree reorganization that introduced this gate: unifying docs/adr/ + .

## Source decision

A fourth doc-sync gate, verify-md-links (scripts/verify-md-links.ts), mirroring the verify-md-wrap style (tsx ESM, AST-based, verify-don't-generate): Parse each in-scope Markdown file with mdast-util-from-markdown + GFM and walk every link, image, and definition node. Check a target only when it is a relative path. Skip scheme-qualified URLs (https:, mailto:, …), protocol-relative (//host), root-absolute (/path --- no stable base in a checkout), and pure in-page anchors (#section). Strip any #fragment/?query, resolve the path against the linking file's directory, and assert it exists on disk.

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/process/2026-06-18-markdown-cross-link-lint.md](../02-notes/implemented/process/2026-06-18-markdown-cross-link-lint.md)
- Pinned source: [.agents/notes/implemented/process/2026-06-18-markdown-cross-link-lint.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/process/2026-06-18-markdown-cross-link-lint.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`docs/AGENTS.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/AGENTS.md) | package contract and examples | The source note names this file directly. Contains the exact code literal `packages/AGENTS.md` named by the note. | `exact-code-occurrence, named-file` |
| [`packages/AGENTS.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/AGENTS.md) | package contract and examples | The source note names this file directly. | `named-file` |
| [`scripts/verify-md-links.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/verify-md-links.ts) | repository automation | The source note names this file directly. Contains the exact code literal `packages/AGENTS.md` named by the note. | `exact-code-occurrence, named-file` |
| [`.agents/notes`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/.agents/notes) | package or module directory | The source note names this implementation area directly. | `named-directory` |
| [`.agents/skills`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/.agents/skills) | package or module directory | The source note names this implementation area directly. | `named-directory` |
| [`scripts/gen-doc-graphs.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/gen-doc-graphs.ts) | repository automation | Defines `link`, a construct named by the note. | `symbol-definition` |
| [`scripts/verify-node-next-types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/verify-node-next-types.ts) | repository automation | Defines `link`, a construct named by the note. | `symbol-definition` |
| [`packages/skill/skill/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/skill/skill/src/index.ts) | package entry point | Defines `definition`, a construct named by the note. | `symbol-definition` |
| [`packages/core/tools/src/code-mode.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/code-mode.ts) | runtime implementation | Defines `definition`, a construct named by the note. | `symbol-definition` |
| [`packages/boot/app-boot/src/profile.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/boot/app-boot/src/profile.ts) | runtime implementation | Defines `link`, a construct named by the note. | `symbol-definition` |
| [`packages/lsp/lsp-stdio/src/translate.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/lsp/lsp-stdio/src/translate.ts) | runtime implementation | Defines `link`, a construct named by the note. | `symbol-definition` |
| [`packages/typert/registry/src/service.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/typert/registry/src/service.ts) | runtime implementation | Defines `definition`, a construct named by the note. | `symbol-definition` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `image` | `const` | [`packages/attachment/attachment-local/src/image.ts:55`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/attachment/attachment-local/src/image.ts#L55) | `const image = sharp(data, { failOn: 'error', limitInputPixels: false })` |
| `link` | `const` | [`packages/boot/app-boot/src/profile.ts:251`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/boot/app-boot/src/profile.ts#L251) | `const link = join(modulesDir, packageName)` |
| `definition` | `const` | [`packages/core/tools/src/code-mode.ts:296`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/code-mode.ts#L296) | `const definition = defineTool({` |
| `definition` | `const` | [`packages/extensions/cordis-host-runner/src/index.ts:186`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/extensions/cordis-host-runner/src/index.ts#L186) | `const definition: DynamicCordisDefinition = {` |
| `link` | `const` | [`packages/lsp/lsp-stdio/src/translate.ts:157`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/lsp/lsp-stdio/src/translate.ts#L157) | `const link = record as unknown as WireLocationLink` |
| `definition` | `const` | [`packages/skill/skill/src/index.ts:448`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/skill/skill/src/index.ts#L448) | `const definition: SkillDefinition = {` |
| `definition` | `const` | [`packages/skill/skill/src/index.ts:507`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/skill/skill/src/index.ts#L507) | `const definition = await waitWithAbort(` |
| `link` | `const` | [`packages/typert/generator/src/analyzer.ts:2426`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/typert/generator/src/analyzer.ts#L2426) | `const link: CrossFaceLink = {` |
| `definition` | `const` | [`packages/typert/registry/src/service.ts:297`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/typert/registry/src/service.ts#L297) | `const definition: TypertLookupDefinition = {` |
| `link` | `const` | [`scripts/gen-doc-graphs.ts:1414`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/gen-doc-graphs.ts#L1414) | `const link = graphIndexLink(doc.rel)` |
| `link` | `const` | [`scripts/verify-node-next-types.ts:85`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/verify-node-next-types.ts#L85) | `const link = resolve(nodeModules, ...parts)` |

### Tests and executable evidence

- [`scripts/run-gates.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/run-gates.spec.ts) — A test under the owning area exercises or imports `doc-sync`.
- [`scripts/verify-md-links.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/verify-md-links.spec.ts) — A test under the owning area exercises or imports `verify-md-links`.
- [`scripts/gen-third-party-notices.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/gen-third-party-notices.spec.ts) — A test under the owning area exercises or imports `doc-sync`.

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

- **`rust/four-tool-surface`** — Preserve the four model tools: read, write, patch, and exec. Reuse internal capabilities without automatically exposing new model-facing tools.
- **`rust/process-discipline`** — Treat this as repository or release policy unless it changes a runtime contract. Do not add runtime machinery for a process-only lesson.

### Smallest safe implementation slice

1. Express the semantic contract as Rust types and one owning boundary.
2. Add the smallest adapter or service needed by the existing loop; do not add a new model-facing tool unless the four-tool surface cannot express the job.
3. Record durable evidence before deriving summaries or UI state.
4. Add one happy-path test, one interruption test, and one replay/restart test where state survives a turn.
5. Keep the patch reversible until the source note's strongest failure case is proven in the Rust harness.

## Search handles

- Tags: `class/process`, `concern/discovery-routing`, `concern/evidence`, `concern/ownership`, `domain/build-release`, `domain/protocols`, `lifecycle/implemented`, `mechanism/policy`
- Aliases: `image`, `link`, `definition`, `docs/adr/`, `.agents/notes/`, `proposed/`, `implemented/`, `rejected/`, `doc-sync`, `verify-md-links`, `scripts/verify-md-links.ts`, `verify-md-wrap`, `mdast-util-from-markdown`, `//host`
- Regex: `(?i)(image|link|definition|docs/adr/|\.agents/notes/|proposed/|implemented/|rejected/)`

```bash
rg -n --pcre2 "(?i)(image|link|definition|docs/adr/|\\.agents/notes/|proposed/|implemented/|rejected/)" source-deepseek-harness
rg -l --fixed-strings "class/process" 03-agent-context
```

## Connected notes

- **`source-link`** — [0630. Doc-sync enforcement](0630-doc-sync-enforcement.md): The source note links to this decision directly.
- **`source-link`** — [0436. verify-md-links validates fragment anchors, closing the last dead-link class](0436-verify-md-links-validates-fragment-anchors-closing-the-last-dead-link-cl.md): The source note links to this decision directly.
- **`shares-code-with`** — [0434. Cite committed artifacts, never design-session ordinals](0434-cite-committed-artifacts-never-design-session-ordinals.md): Shares source implementation: `docs/AGENTS.md`, `scripts/gen-doc-graphs.ts`.
- **`shares-code-with`** — [0386. Documentation structure, tiers, and budgets](0386-documentation-structure-tiers-and-budgets.md): Shares source implementation: `docs/AGENTS.md`, `packages/AGENTS.md`.
- **`shares-code-with`** — [0192. Skill catalog hot refresh](0192-skill-catalog-hot-refresh.md): Shares source implementation: `.agents/skills`, `packages/skill/skill/src/index.ts`.
- **`shares-code-with`** — [0391. A gated Known-Limitations section in every package README](0391-a-gated-known-limitations-section-in-every-package-readme.md): Shares source implementation: `docs/AGENTS.md`, `packages/AGENTS.md`.
- **`shares-code-with`** — [0382. Classify Agent Notes by kind via path-encoded subdirectories](0382-classify-agent-notes-by-kind-via-path-encoded-subdirectories.md): Shares source implementation: `.agents/notes`.
- **`shares-code-with`** — [0358. The minimal preset owns the complete RL agent composition](0358-the-minimal-preset-owns-the-complete-rl-agent-composition.md): Shares source implementation: `scripts/gen-doc-graphs.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0381-markdown-cross-link-validity-linting.md`.
