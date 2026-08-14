---
id: "dsh-note-0425"
title: "The documentation site carries its own images"
status: "implemented"
class: "process"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/process/2026-08-06-doc-site-carries-its-images.md"
implementation_evidence: "high"
target_anchor: "repository tests and engineering policy"
tags:
  - "class/process"
  - "concern/boundary"
  - "concern/discovery-routing"
  - "concern/evidence"
  - "concern/ownership"
  - "concern/recovery"
  - "concern/simplification"
  - "domain/build-release"
  - "domain/filesystem"
  - "domain/llm"
  - "domain/session-state"
  - "domain/testing"
  - "domain/web-retrieval"
  - "lifecycle/implemented"
  - "mechanism/policy"
  - "mechanism/projection"
aliases:
  - "rewriteMarkdown"
  - "publishableImage"
  - "docsSourceFiles"
  - "scripts/project-doc-site.ts"
  - "https://raw.githubusercontent.com/<owner>/<repo>/<ref>/<path>"
  - "srcDir"
  - ".generated"
  - "publicDir"
  - "<srcDir>/public"
  - "raw.githubusercontent.com"
  - "verify-md-links"
  - "placeImage(absPath): string"
  - "placeImage"
  - "./<basename>"
search_regex: "(?i)(rewriteMarkdown|publishableImage|docsSourceFiles|scripts/project\\-doc\\-site\\.ts|srcDir|\\.generated|publicDir|<srcDir>/public)"
---

# 0425. The documentation site carries its own images — implementation context

## Open this when

scripts/project-doc-site.ts rewrote every repository-relative target that the publication manifest does not publish into a GitHub URL, and for an image that meant https://raw.githubusercontent.com////. Nothing in the site build copies files: srcDir is the disposable .generated tree, VitePress sets no publicDir (its default, /public, is inside the tree the projector deletes on every run), and only Markdown is written there. That works only for a public repository.

## Source decision

rewriteMarkdown takes an optional placeImage(absPath): string. When a page references an image the manifest does not publish as a page, the projector copies that file into the generated tree beside the page and rewrites the reference to ./; Vite then bundles it like any other site asset. Nothing about repository visibility can reach the published page. The copy lands beside the page rather than in a shared asset directory.

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/process/2026-08-06-doc-site-carries-its-images.md](../02-notes/implemented/process/2026-08-06-doc-site-carries-its-images.md)
- Pinned source: [.agents/notes/implemented/process/2026-08-06-doc-site-carries-its-images.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/process/2026-08-06-doc-site-carries-its-images.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`scripts/project-doc-site.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/project-doc-site.ts) | repository automation | The source note names this file directly. Defines `rewriteMarkdown`, a construct named by the note. | `named-file, symbol-definition` |
| [`package.json`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/package.json) | composition and configuration | Contains the exact code literal `scripts/project-doc-site.spec.ts` named by the note. | `exact-code-occurrence` |
| [`scripts/run-gates.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/run-gates.ts) | repository automation | Contains the exact code literal `scripts/project-doc-site.spec.ts` named by the note. | `exact-code-occurrence` |
| [`website/.vitepress/config.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/website/.vitepress/config.ts) | runtime implementation | Contains the exact code literal `scripts/project-doc-site.ts` named by the note. Contains the exact code literal `en/guide/` named by the note. | `exact-code-occurrence` |
| [`.github/workflows/docs-pages.yml`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.github/workflows/docs-pages.yml) | repository automation | Contains the exact code literal `scripts/project-doc-site.ts` named by the note. Contains the exact code literal `scripts/project-doc-site.spec.ts` named by the note. | `exact-code-occurrence` |
| [`.agents/skills/dsh-doc-site-sync/SKILL.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/skills/dsh-doc-site-sync/SKILL.md) | package contract and examples | Contains the exact code literal `scripts/project-doc-site.ts` named by the note. | `exact-code-occurrence` |
| [`scripts/snapshots/translation-prompt-v4/request-response.expected.json`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/snapshots/translation-prompt-v4/request-response.expected.json) | repository automation | Contains the exact code literal `scripts/project-doc-site.spec.ts` named by the note. | `exact-code-occurrence` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `rewriteMarkdown` | `function` | [`scripts/project-doc-site.ts:225`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/project-doc-site.ts#L225) | `export function rewriteMarkdown(source: string, options: RewriteMarkdownOptions): string {` |
| `publishableImage` | `function` | [`scripts/project-doc-site.ts:359`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/project-doc-site.ts#L359) | `export function publishableImage(absPath: string, repoRoot: string): string \| undefined {` |
| `docsSourceFiles` | `function` | [`scripts/project-doc-site.ts:394`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/project-doc-site.ts#L394) | `export function docsSourceFiles(): string[] {` |

### Tests and executable evidence

- [`scripts/project-doc-site.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/project-doc-site.spec.ts) — The source note names this file directly. A test under the owning area exercises or imports `com`.
- [`scripts/change-scope.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/change-scope.spec.ts) — A test under the owning area exercises or imports `com`.
- [`scripts/verify-md-links.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/verify-md-links.spec.ts) — A test under the owning area exercises or imports `verify-md-links`.
- [`scripts/translation-pairing.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/translation-pairing.spec.ts) — A test under the owning area exercises or imports `com`.
- [`apps/cli/tests/dsh-badge.snapshot.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/cli/tests/dsh-badge.snapshot.ts) — A test under the owning area exercises or imports `com`.
- [`scripts/gen-third-party-notices.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/gen-third-party-notices.spec.ts) — A test under the owning area exercises or imports `com`.
- [`scripts/verify-doc-site-fragments.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/verify-doc-site-fragments.spec.ts) — A test under the owning area exercises or imports `com`.
- [`scripts/verify-public-repository-links.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/verify-public-repository-links.spec.ts) — A test under the owning area exercises or imports `com`.
- Source verification intent: scripts/project-doc-site.spec.ts covers the placer receiving the resolved absolute path and the returned URL landing in the Markdown, a placed reference keeping its fragment, a published page link still resolving to its route when a placer is present, and the unchanged GitHub-raw fallback when no placer is supplied. publishableImage is covered directly: a regular file inside the repository resolves, while a symlink whose target escapes it, a path outside it, and a directory are all refused.

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
- **`rust/process-discipline`** — Treat this as repository or release policy unless it changes a runtime contract. Do not add runtime machinery for a process-only lesson.
- **`rust/provider-adapter`** — Keep provider wire quirks inside adapters; the loop and durable event vocabulary remain provider-neutral.
- **`rust/trait-seam`** — Use a small trait at the capability boundary; keep policy and model-facing schemas outside the backend implementation.

### Smallest safe implementation slice

1. Express the semantic contract as Rust types and one owning boundary.
2. Add the smallest adapter or service needed by the existing loop; do not add a new model-facing tool unless the four-tool surface cannot express the job.
3. Record durable evidence before deriving summaries or UI state.
4. Add one happy-path test, one interruption test, and one replay/restart test where state survives a turn.
5. Keep the patch reversible until the source note's strongest failure case is proven in the Rust harness.

## Search handles

- Tags: `class/process`, `concern/boundary`, `concern/discovery-routing`, `concern/evidence`, `concern/ownership`, `concern/recovery`, `concern/simplification`, `domain/build-release`, `domain/filesystem`, `domain/llm`, `domain/session-state`, `domain/testing`, `domain/web-retrieval`, `lifecycle/implemented`, `mechanism/policy`, `mechanism/projection`
- Aliases: `rewriteMarkdown`, `publishableImage`, `docsSourceFiles`, `scripts/project-doc-site.ts`, `https://raw.githubusercontent.com/<owner>/<repo>/<ref>/<path>`, `srcDir`, `.generated`, `publicDir`, `<srcDir>/public`, `raw.githubusercontent.com`, `verify-md-links`, `placeImage(absPath): string`, `placeImage`, `./<basename>`
- Regex: `(?i)(rewriteMarkdown|publishableImage|docsSourceFiles|scripts/project\-doc\-site\.ts|srcDir|\.generated|publicDir|<srcDir>/public)`

```bash
rg -n --pcre2 "(?i)(rewriteMarkdown|publishableImage|docsSourceFiles|scripts/project\\-doc\\-site\\.ts|srcDir|\\.generated|publicDir|<srcDir>/public)" source-deepseek-harness
rg -l --fixed-strings "class/process" 03-agent-context
```

## Connected notes

- **`shares-code-with`** — [0393. Project canonical documentation into the website](0393-project-canonical-documentation-into-the-website.md): Shares source implementation: `scripts/gen-third-party-notices.spec.ts`, `scripts/project-doc-site.spec.ts`.
- **`shares-code-with`** — [0406. Briefed minimal translation updates](0406-briefed-minimal-translation-updates.md): Shares source implementation: `scripts/gen-third-party-notices.spec.ts`, `scripts/translation-pairing.spec.ts`.
- **`shares-code-with`** — [0392. Package Model Experience contract](0392-package-model-experience-contract.md): Shares source implementation: `scripts/gen-third-party-notices.spec.ts`, `scripts/project-doc-site.ts`.
- **`shares-code-with`** — [0644. Personal staging maintenance skills](0644-personal-staging-maintenance-skills.md): Shares source implementation: `scripts/change-scope.spec.ts`, `scripts/translation-pairing.spec.ts`.
- **`shares-code-with`** — [0442. Documentation-site navigation and repository chrome](0442-documentation-site-navigation-and-repository-chrome.md): Shares source implementation: `scripts/project-doc-site.spec.ts`, `scripts/project-doc-site.ts`.
- **`shares-code-with`** — [0415. Make Lefthook installation worktree-local](0415-make-lefthook-installation-worktree-local.md): Shares source implementation: `scripts/change-scope.spec.ts`, `scripts/translation-pairing.spec.ts`.
- **`shares-code-with`** — [0374. Unlink stale profile fallback links instead of rmSync](0374-unlink-stale-profile-fallback-links-instead-of-rmsync.md): Shares source implementation: `scripts/change-scope.spec.ts`, `scripts/verify-md-links.spec.ts`.
- **`shares-code-with`** — [0526. Remove the packed-session fixture branch migrator](0526-remove-the-packed-session-fixture-branch-migrator.md): Shares source implementation: `scripts/change-scope.spec.ts`, `scripts/translation-pairing.spec.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0425-the-documentation-site-carries-its-own-images.md`.
