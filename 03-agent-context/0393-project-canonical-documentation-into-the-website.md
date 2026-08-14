---
id: "dsh-note-0393"
title: "Project canonical documentation into the website"
status: "implemented"
class: "process"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/process/2026-07-13-documentation-site-projection.md"
implementation_evidence: "high"
target_anchor: "repository tests and engineering policy"
tags:
  - "class/process"
  - "concern/boundary"
  - "concern/compatibility"
  - "concern/discovery-routing"
  - "concern/evidence"
  - "concern/lifecycle"
  - "concern/ownership"
  - "concern/recovery"
  - "concern/schema-types"
  - "concern/trust"
  - "domain/build-release"
  - "domain/extensions"
  - "domain/filesystem"
  - "domain/llm"
  - "domain/security"
  - "domain/session-state"
  - "domain/testing"
  - "domain/ui-interaction"
  - "lifecycle/implemented"
  - "mechanism/adapter"
  - "mechanism/policy"
  - "mechanism/projection"
aliases:
  - "editSource"
  - "docs/user/"
  - "docs/"
  - "website/docs.ts"
  - "scripts/project-doc-site.ts"
  - "website/.generated/"
  - "deepseek-ai/deepseek-harness"
  - "doc-sync"
  - "verify-public-repository-links"
  - "website/AGENTS.md"
  - "vitepress-plugin-mermaid"
  - "website/.dist"
  - "actions/configure-pages"
  - "website/"
search_regex: "(?i)(editSource|docs/user/|docs/|website/docs\\.ts|scripts/project\\-doc\\-site\\.ts|website/\\.generated/|deepseek\\-ai/deepseek\\-harness|doc\\-sync)"
---

# 0393. Project canonical documentation into the website — implementation context

## Open this when

The repository needs a navigable documentation website without turning the website directory into a second documentation source. Copying package guides, architecture pages, or generated catalogs into a site-specific tree allows the two copies to drift, while pointing VitePress directly at the repository root couples public URLs and navigation to the internal file layout. Repository-relative links also need different destinations on the website: published pages stay inside the site, but source files and unpublished contributor documents belong on GitHub.

## Source decision

Canonical Markdown remains in the repository tier that owns it. Product-facing guides live under docs/user/, generated reference remains in the existing generated catalogs, and architectural and cookbook pages remain at their existing docs/ paths. website/docs.ts is an explicit publication manifest. Each entry maps one canonical source file to a stable public route, sidebar, section, and order. Adding or removing a published page is therefore a reviewable manifest change rather than an implicit directory crawl.

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/process/2026-07-13-documentation-site-projection.md](../02-notes/implemented/process/2026-07-13-documentation-site-projection.md)
- Pinned source: [.agents/notes/implemented/process/2026-07-13-documentation-site-projection.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/process/2026-07-13-documentation-site-projection.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`scripts/project-doc-site.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/project-doc-site.ts) | repository automation | The source note names this file directly. Contains the exact code literal `website/docs.ts` named by the note. | `exact-code-occurrence, named-file` |
| [`docs/user`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/docs/user) | package or module directory | The source note names this implementation area directly. | `named-directory` |
| [`website/.vitepress/config.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/website/.vitepress/config.ts) | runtime implementation | Defines `editSource`, a construct named by the note. Contains the exact code literal `scripts/project-doc-site.ts` named by the note. | `exact-code-occurrence, symbol-definition` |
| [`.github/workflows/docs-pages.yml`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.github/workflows/docs-pages.yml) | repository automation | Contains the exact code literal `scripts/project-doc-site.ts` named by the note. Contains the exact code literal `actions/configure-pages` named by the note. | `exact-code-occurrence` |
| [`.agents/skills/dsh-doc-site-sync/SKILL.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/skills/dsh-doc-site-sync/SKILL.md) | package contract and examples | Contains the exact code literal `website/docs.ts` named by the note. Contains the exact code literal `scripts/project-doc-site.ts` named by the note. | `exact-code-occurrence` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `editSource` | `const` | [`website/.vitepress/config.ts:155`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/website/.vitepress/config.ts#L155) | `const editSource: unknown = typeof data === 'object' && data !== null ? Reflect.get(data, 'editSource') : undefined` |
| `editSource` | `const` | [`website/.vitepress/config.ts:307`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/website/.vitepress/config.ts#L307) | `const editSource: unknown = typeof data === 'object' && data !== null ? Reflect.get(data, 'editSource') : undefined` |

### Tests and executable evidence

- [`scripts/run-gates.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/run-gates.spec.ts) — A test under the owning area exercises or imports `doc-sync`.
- [`scripts/gen-third-party-notices.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/gen-third-party-notices.spec.ts) — A test under the owning area exercises or imports `doc-sync`.
- [`scripts/verify-public-repository-links.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/verify-public-repository-links.spec.ts) — A test under the owning area exercises or imports `verify-public-repository-links`.
- [`scripts/project-doc-site.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/project-doc-site.spec.ts) — Contains the exact code literal `website/docs.ts` named by the note. Contains the exact code literal `website/AGENTS.md` named by the note.

## How to read the implementation

1. Start with [`scripts/project-doc-site.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/project-doc-site.ts) because it has the strongest evidence link to the note.
2. Read public types and entry points before implementation details. Write down the contract in plain language.
3. Follow the named constructs into their callers and tests. Confirm which component can actually enforce the rule.
4. Reconstruct one failure path: cancellation, crash, retry, stale state, partial write, or incompatible provider input—whichever the note is protecting against.
5. Only then map the contract into the current Rust repository. Target paths must come from that repository, never from guesswork in this wiki.

## Rust harness port

**Likely integration point:** repository tests and engineering policy.

Preserve the behavior and ownership rule. Do not mechanically translate Cordis, TypeScript declaration merging, Node streams, or package topology into Rust.

Relevant standing rules:

- **`rust/append-log`** — Keep durable facts append-only and derive views from them. Separate the synchronous fact boundary from asynchronous persistence.
- **`rust/capability-security`** — Make granted capabilities explicit. On the Windows no-admin path, probe support at runtime and fail closed rather than silently running unsandboxed.
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

- Tags: `class/process`, `concern/boundary`, `concern/compatibility`, `concern/discovery-routing`, `concern/evidence`, `concern/lifecycle`, `concern/ownership`, `concern/recovery`, `concern/schema-types`, `concern/trust`, `domain/build-release`, `domain/extensions`, `domain/filesystem`, `domain/llm`, `domain/security`, `domain/session-state`, `domain/testing`, `domain/ui-interaction`, `lifecycle/implemented`, `mechanism/adapter`, `mechanism/policy`, `mechanism/projection`
- Aliases: `editSource`, `docs/user/`, `docs/`, `website/docs.ts`, `scripts/project-doc-site.ts`, `website/.generated/`, `deepseek-ai/deepseek-harness`, `doc-sync`, `verify-public-repository-links`, `website/AGENTS.md`, `vitepress-plugin-mermaid`, `website/.dist`, `actions/configure-pages`, `website/`
- Regex: `(?i)(editSource|docs/user/|docs/|website/docs\.ts|scripts/project\-doc\-site\.ts|website/\.generated/|deepseek\-ai/deepseek\-harness|doc\-sync)`

```bash
rg -n --pcre2 "(?i)(editSource|docs/user/|docs/|website/docs\\.ts|scripts/project\\-doc\\-site\\.ts|website/\\.generated/|deepseek\\-ai/deepseek\\-harness|doc\\-sync)" source-deepseek-harness
rg -l --fixed-strings "class/process" 03-agent-context
```

## Connected notes

- **`source-link`** — [0425. The documentation site carries its own images](0425-the-documentation-site-carries-its-own-images.md): The source note links to this decision directly.
- **`source-link`** — [0488. Route documentation roots to quick start](0488-route-documentation-roots-to-quick-start.md): The source note links to this decision directly.
- **`shares-code-with`** — [0392. Package Model Experience contract](0392-package-model-experience-contract.md): Shares source implementation: `scripts/gen-third-party-notices.spec.ts`, `scripts/project-doc-site.ts`.
- **`shares-code-with`** — [0442. Documentation-site navigation and repository chrome](0442-documentation-site-navigation-and-repository-chrome.md): Shares source implementation: `scripts/project-doc-site.spec.ts`, `scripts/project-doc-site.ts`.
- **`shares-code-with`** — [0441. Python public publication workflow](0441-python-public-publication-workflow.md): Shares source implementation: `scripts/project-doc-site.spec.ts`, `scripts/run-gates.spec.ts`.
- **`shares-code-with`** — [0430. Lightweight routine documentation translation](0430-lightweight-routine-documentation-translation.md): Shares source implementation: `scripts/gen-third-party-notices.spec.ts`, `scripts/run-gates.spec.ts`.
- **`shares-code-with`** — [0431. Dual Wine and native Windows pull-request CI](0431-dual-wine-and-native-windows-pull-request-ci.md): Shares source implementation: `scripts/run-gates.spec.ts`.
- **`shares-code-with`** — [0406. Briefed minimal translation updates](0406-briefed-minimal-translation-updates.md): Shares source implementation: `scripts/gen-third-party-notices.spec.ts`, `scripts/run-gates.spec.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0393-project-canonical-documentation-into-the-website.md`.
