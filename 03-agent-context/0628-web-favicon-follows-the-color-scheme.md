---
id: "dsh-note-0628"
title: "Web favicon follows the color scheme"
status: "archived"
class: "bug-fix"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/archived/bug-fix/2026-08-10-web-favicon-dark-mode.md"
implementation_evidence: "high"
target_anchor: "repository tests and release policy"
tags:
  - "class/bug-fix"
  - "concern/evidence"
  - "concern/ownership"
  - "concern/recovery"
  - "domain/build-release"
  - "domain/web-retrieval"
  - "lifecycle/archived"
  - "mechanism/projection"
aliases:
  - "html"
  - "theme"
  - "apps/web/public/favicon.svg"
  - "index.html"
  - "favicon.svg"
  - "@media (prefers-color-scheme: dark) { path { fill: #fff } }"
  - "manifest.webmanifest"
  - "favicon-32x32.png"
  - "#4D6BFE"
  - "dsh.theme"
  - "prefers-color-scheme"
  - "<link rel=\"icon\" media=\"(prefers-color-scheme: dark)\">"
  - "theme/change"
  - "apps/web/tests/pwa-manifest.e2e.ts"
search_regex: "(?i)(html|theme|apps/web/public/favicon\\.svg|index\\.html|favicon\\.svg|manifest\\.webmanifest|favicon\\-32x32\\.png|\\#4D6BFE)"
---

# 0628. Web favicon follows the color scheme — implementation context

## Open this when

apps/web/public/favicon.svg paints the DeepSeek mark solid black (fill="#000"), and index.html declares only that single SVG icon. Under an OS or browser dark color scheme the tab strip is dark too, so the black mark is effectively invisible. Safari versions before 26 do not render SVG favicons, so their users get no tab icon in any scheme.

## Source decision

The favicon stays one file and adapts through the browser's own color-scheme signal: favicon.svg embeds @media (prefers-color-scheme: dark) { path { fill: #fff } }, switching the mark to white under a dark scheme while the light scheme keeps black. index.html and manifest.webmanifest also declare a 32×32 PNG fallback (favicon-32x32.png, DeepSeek brand blue #4D6BFE) that Safari versions before 26 render and that stays visible on both light and dark tab strips, extending the web-install-manifest decision.

## Decision status

Historical design. Use it to understand an older pressure or failure; locate the replacing decision before implementing anything.

- Raw note: [archived/bug-fix/2026-08-10-web-favicon-dark-mode.md](../02-notes/archived/bug-fix/2026-08-10-web-favicon-dark-mode.md)
- Pinned source: [.agents/notes/archived/bug-fix/2026-08-10-web-favicon-dark-mode.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/archived/bug-fix/2026-08-10-web-favicon-dark-mode.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`scripts/verify-md-links.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/verify-md-links.ts) | repository automation | Defines `html`, a construct named by the note. | `symbol-definition` |
| [`packages/client/ui-theme/src/client/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-theme/src/client/index.ts) | package entry point | Defines `theme`, a construct named by the note. | `symbol-definition` |
| [`packages/client/ui-primitives/src/markdown/CodeBlock.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-primitives/src/markdown/CodeBlock.tsx) | runtime implementation | Defines `html`, a construct named by the note. | `symbol-definition` |
| [`packages/extensions/cordis-client-runner/src/client/providers.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/extensions/cordis-client-runner/src/client/providers.ts) | provider/backend adapter | Defines `theme`, a construct named by the note. | `symbol-definition` |
| [`scripts/gen-cordis-catalog.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/gen-cordis-catalog.ts) | repository automation | Contains the exact code literal `theme/change` named by the note. | `exact-code-occurrence` |
| [`packages/client/ui-theme/README.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-theme/README.md) | package contract and examples | Contains the exact code literal `theme/change` named by the note. | `exact-code-occurrence` |
| [`scripts/gen-cordis-inspect-catalog.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/gen-cordis-inspect-catalog.ts) | repository automation | Contains the exact code literal `theme/change` named by the note. | `exact-code-occurrence` |
| [`packages/client/ui-theme/README.zh.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-theme/README.zh.md) | package contract and examples | Contains the exact code literal `theme/change` named by the note. | `exact-code-occurrence` |
| [`packages/client/ui-layout/src/client/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-layout/src/client/index.ts) | package entry point | Contains the exact code literal `theme/change` named by the note. | `exact-code-occurrence` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `html` | `const` | [`packages/client/ui-primitives/src/markdown/CodeBlock.tsx:32`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-primitives/src/markdown/CodeBlock.tsx#L32) | `const html = useMemo(() => highlightToHtml(trimmed, lang), [trimmed, lang, loaded])` |
| `theme` | `const` | [`packages/client/ui-theme/src/client/index.ts:386`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-theme/src/client/index.ts#L386) | `const theme = new ThemeRuntime(ctx, host)` |
| `theme` | `const` | [`packages/extensions/cordis-client-runner/src/client/providers.ts:127`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/extensions/cordis-client-runner/src/client/providers.ts#L127) | `const theme = ctx.get('theme')` |
| `html` | `const` | [`scripts/verify-md-links.ts:131`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/verify-md-links.ts#L131) | `const html = node.value.replace(/<!--[\s\S]*?-->/g, '')` |

### Tests and executable evidence

- [`apps/web/tests/pwa-manifest.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/pwa-manifest.e2e.ts) — The source note names this file directly. A test under the owning area exercises or imports `svg`.
- [`scripts/project-doc-site.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/project-doc-site.spec.ts) — A test under the owning area exercises or imports `svg`.
- [`apps/web/tests/seeded-history.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/seeded-history.e2e.ts) — A test under the owning area exercises or imports `svg`.
- [`apps/web/tests/produced-files.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/produced-files.e2e.ts) — A test under the owning area exercises or imports `svg`.
- [`apps/web/tests/startup-auto-selection.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/startup-auto-selection.e2e.ts) — A test under the owning area exercises or imports `svg`.
- [`packages/client/ui-theme/tests/host.client.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-theme/tests/host.client.spec.ts) — A test under the owning area exercises or imports `theme`.
- [`packages/client/ui-theme/tests/theme.client.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-theme/tests/theme.client.spec.ts) — A test under the owning area exercises or imports `prefers-color-scheme`.
- [`packages/client/ui-tool/tests/todo-row.client.spec.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-tool/tests/todo-row.client.spec.tsx) — A test under the owning area exercises or imports `svg`.

## How to read the implementation

1. Start with [`scripts/verify-md-links.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/verify-md-links.ts) because it has the strongest evidence link to the note.
2. Read public types and entry points before implementation details. Write down the contract in plain language.
3. Follow the named constructs into their callers and tests. Confirm which component can actually enforce the rule.
4. Reconstruct one failure path: cancellation, crash, retry, stale state, partial write, or incompatible provider input—whichever the note is protecting against.
5. Only then map the contract into the current Rust repository. Target paths must come from that repository, never from guesswork in this wiki.

## Rust harness port

**Likely integration point:** repository tests and release policy.

Preserve the behavior and ownership rule. Do not mechanically translate Cordis, TypeScript declaration merging, Node streams, or package topology into Rust.

Relevant standing rules:

- **`rust/four-tool-surface`** — Preserve the four model tools: read, write, patch, and exec. Reuse internal capabilities without automatically exposing new model-facing tools.
- **`rust/process-discipline`** — Treat this as repository or release policy unless it changes a runtime contract. Do not add runtime machinery for a process-only lesson.
- **Status gate** — Historical design. Use it to understand an older pressure or failure; locate the replacing decision before implementing anything.

### Smallest safe implementation slice

1. Express the semantic contract as Rust types and one owning boundary.
2. Add the smallest adapter or service needed by the existing loop; do not add a new model-facing tool unless the four-tool surface cannot express the job.
3. Record durable evidence before deriving summaries or UI state.
4. Add one happy-path test, one interruption test, and one replay/restart test where state survives a turn.
5. Keep the patch reversible until the source note's strongest failure case is proven in the Rust harness.

## Search handles

- Tags: `class/bug-fix`, `concern/evidence`, `concern/ownership`, `concern/recovery`, `domain/build-release`, `domain/web-retrieval`, `lifecycle/archived`, `mechanism/projection`
- Aliases: `html`, `theme`, `apps/web/public/favicon.svg`, `index.html`, `favicon.svg`, `@media (prefers-color-scheme: dark) { path { fill: #fff } }`, `manifest.webmanifest`, `favicon-32x32.png`, `#4D6BFE`, `dsh.theme`, `prefers-color-scheme`, `<link rel="icon" media="(prefers-color-scheme: dark)">`, `theme/change`, `apps/web/tests/pwa-manifest.e2e.ts`
- Regex: `(?i)(html|theme|apps/web/public/favicon\.svg|index\.html|favicon\.svg|manifest\.webmanifest|favicon\-32x32\.png|\#4D6BFE)`

```bash
rg -n --pcre2 "(?i)(html|theme|apps/web/public/favicon\\.svg|index\\.html|favicon\\.svg|manifest\\.webmanifest|favicon\\-32x32\\.png|\\#4D6BFE)" source-deepseek-harness
rg -l --fixed-strings "class/bug-fix" 03-agent-context
```

## Connected notes

- **`shares-code-with`** — [0108. Web shell dist chunk split and directory layout](0108-web-shell-dist-chunk-split-and-directory-layout.md): Shares source implementation: `packages/client/ui-primitives/src/markdown/CodeBlock.tsx`, `scripts/verify-md-links.ts`.
- **`shares-code-with`** — [0314. Web GUI changes close the loop on the existing URL](0314-web-gui-changes-close-the-loop-on-the-existing-url.md): Shares source implementation: `packages/client/ui-primitives/src/markdown/CodeBlock.tsx`, `scripts/verify-md-links.ts`.
- **`shares-code-with`** — [0267. Resolved theme color metadata](0267-resolved-theme-color-metadata.md): Shares source implementation: `apps/web/tests/pwa-manifest.e2e.ts`, `packages/client/ui-theme/src/client/index.ts`.
- **`shares-code-with`** — [0618. Question-composer option rows are scroll content, not the slack absorber](0618-question-composer-option-rows-are-scroll-content-not-the-slack-absorber.md): Shares source implementation: `apps/web/tests/produced-files.e2e.ts`, `apps/web/tests/seeded-history.e2e.ts`.
- **`shares-code-with`** — [0605. Web composer stats detail and input-zone polish](0605-web-composer-stats-detail-and-input-zone-polish.md): Shares source implementation: `scripts/project-doc-site.spec.ts`.
- **`shares-code-with`** — [0412. Web client syntax highlighting --- synchronous fine-grained shiki](0412-web-client-syntax-highlighting-synchronous-fine-grained-shiki.md): Shares source implementation: `packages/client/ui-primitives/src/markdown/CodeBlock.tsx`.
- **`shares-code-with`** — [0177. Safe assistant Markdown in the Web conversation](0177-safe-assistant-markdown-in-the-web-conversation.md): Shares source implementation: `packages/client/ui-primitives/src/markdown/CodeBlock.tsx`.
- **`shares-code-with`** — [0425. The documentation site carries its own images](0425-the-documentation-site-carries-its-own-images.md): Shares source implementation: `scripts/project-doc-site.spec.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0628-web-favicon-follows-the-color-scheme.md`.
