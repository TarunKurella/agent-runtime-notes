---
id: "dsh-note-0108"
title: "Web shell dist chunk split and directory layout"
status: "implemented"
class: "architecture"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/architecture/2026-08-06-web-shell-dist-chunk-layout.md"
implementation_evidence: "high"
target_anchor: "read/write/patch filesystem capability"
tags:
  - "class/architecture"
  - "concern/boundary"
  - "concern/evidence"
  - "concern/ownership"
  - "concern/performance"
  - "concern/recovery"
  - "domain/build-release"
  - "domain/configuration"
  - "domain/extensions"
  - "domain/filesystem"
  - "domain/jobs-tasks"
  - "domain/session-state"
  - "domain/shell-terminal"
  - "domain/testing"
  - "domain/ui-interaction"
  - "domain/web-retrieval"
  - "lifecycle/implemented"
  - "mechanism/event-sourcing"
  - "mechanism/projection"
  - "mechanism/streaming"
aliases:
  - "VENDOR_PACKAGES"
  - "BOOT_GRAMMAR_FILES"
  - "FONT_EXTENSIONS"
  - "html"
  - "LAZY_GRAMMARS"
  - "vendor"
  - "dist/assets/"
  - "apps/web/vite.config.ts"
  - "manualChunks"
  - "@shikijs/langs"
  - "index.html"
  - "chunkFileNames"
  - "assetFileNames"
  - "assets/"
search_regex: "(?i)(VENDOR_PACKAGES|BOOT_GRAMMAR_FILES|FONT_EXTENSIONS|html|LAZY_GRAMMARS|vendor|dist/assets/|apps/web/vite\\.config\\.ts)"
---

# 0108. Web shell dist chunk split and directory layout — implementation context

## Open this when

The apps/web shell previously built into a single ~1.2 MB (minified) index chunk, roughly 80% of it vendor bytes --- KaTeX, the boot grammars and the shiki engine, react-dom, the markdown pipeline --- fused with all the workspace shell code (about one fifth). Any one-line shell change rehashed the whole chunk, forcing returning clients to redownload everything; dist/assets/ was a flat single-level spread of 100-plus files (the main chunk, 23 lazy-loaded grammar chunks, 59 KaTeX font faces, and sourcemaps intermixed), impossible to navigate.

## Source decision

apps/web/vite.config.ts splits the shell into two initial chunks via manualChunks and sorts the output into directories via naming functions; the entire configuration contains zero regexes --- an exact-package-name Set, a filename list, an extension list. Membership (VENDOR_PACKAGES, by exact npm package name): vendor = the three heavy rendering families: math (katex), highlight (shiki), markdown (the micromark/mdast parse pipeline --- the incremental React renderer above it is workspace code and not part of this).

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/architecture/2026-08-06-web-shell-dist-chunk-layout.md](../02-notes/implemented/architecture/2026-08-06-web-shell-dist-chunk-layout.md)
- Pinned source: [.agents/notes/implemented/architecture/2026-08-06-web-shell-dist-chunk-layout.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/architecture/2026-08-06-web-shell-dist-chunk-layout.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`apps/web/vite.config.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/vite.config.ts) | runtime implementation | Entry point or contract under the directory named by the note: `apps/web`. The source note names this file directly. | `exact-code-occurrence, named-directory-member, named-file, symbol-definition` |
| [`scripts/attribute-chunk-bytes.mjs`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/attribute-chunk-bytes.mjs) | repository automation | The source note names this file directly. Defines `vendor`, a construct named by the note. | `named-file, symbol-definition` |
| [`apps/web/package.json`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/package.json) | composition and configuration | Entry point or contract under the directory named by the note: `apps/web`. | `named-directory-member` |
| [`apps/web`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/apps/web) | package or module directory | The source note names this implementation area directly. | `named-directory` |
| [`scripts/verify-md-links.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/verify-md-links.ts) | repository automation | Defines `html`, a construct named by the note. | `symbol-definition` |
| [`packages/client/ui-primitives/src/markdown/highlight.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-primitives/src/markdown/highlight.ts) | runtime implementation | Defines `LAZY_GRAMMARS`, a construct named by the note. | `symbol-definition` |
| [`packages/client/ui-primitives/src/markdown/CodeBlock.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-primitives/src/markdown/CodeBlock.tsx) | runtime implementation | Defines `html`, a construct named by the note. | `symbol-definition` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `VENDOR_PACKAGES` | `const` | [`apps/web/vite.config.ts:41`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/vite.config.ts#L41) | `const VENDOR_PACKAGES: ReadonlySet<string> = new Set([` |
| `BOOT_GRAMMAR_FILES` | `const` | [`apps/web/vite.config.ts:70`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/vite.config.ts#L70) | `const BOOT_GRAMMAR_FILES: readonly string[] = [` |
| `FONT_EXTENSIONS` | `const` | [`apps/web/vite.config.ts:77`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/vite.config.ts#L77) | `const FONT_EXTENSIONS: readonly string[] = ['.woff2', '.woff', '.ttf']` |
| `html` | `const` | [`packages/client/ui-primitives/src/markdown/CodeBlock.tsx:32`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-primitives/src/markdown/CodeBlock.tsx#L32) | `const html = useMemo(() => highlightToHtml(trimmed, lang), [trimmed, lang, loaded])` |
| `LAZY_GRAMMARS` | `const` | [`packages/client/ui-primitives/src/markdown/highlight.ts:53`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-primitives/src/markdown/highlight.ts#L53) | `const LAZY_GRAMMARS = new Map<string, () => Promise<LangModule>>([` |
| `vendor` | `let` | [`scripts/attribute-chunk-bytes.mjs:95`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/attribute-chunk-bytes.mjs#L95) | `let vendor = 0, ws = 0, other = 0` |
| `html` | `const` | [`scripts/verify-md-links.ts:131`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/verify-md-links.ts#L131) | `const html = node.value.replace(/<!--[\s\S]*?-->/g, '')` |

### Tests and executable evidence

- [`scripts/cordis-core-api.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/cordis-core-api.spec.ts) — A test under the owning area exercises or imports `vendor`.
- [`scripts/cordis-config-files.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/cordis-config-files.spec.ts) — A test under the owning area exercises or imports `vendor`.
- [`scripts/translation-pairing.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/translation-pairing.spec.ts) — A test under the owning area exercises or imports `vendor`.
- [`apps/web/tests/math-rendering.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/math-rendering.e2e.ts) — A test under the owning area exercises or imports `katex`.
- [`scripts/gen-third-party-notices.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/gen-third-party-notices.spec.ts) — A test under the owning area exercises or imports `vendor`.
- [`scripts/verify-dsh-package-licenses.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/verify-dsh-package-licenses.spec.ts) — A test under the owning area exercises or imports `vendor`.
- [`packages/client/ui-primitives/tests/markdown.client.spec.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-primitives/tests/markdown.client.spec.tsx) — A test under the owning area exercises or imports `html`. A test under the owning area exercises or imports `katex`.
- [`packages/client/ui-primitives/tests/code-block.client.spec.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-primitives/tests/code-block.client.spec.tsx) — A test under the owning area exercises or imports `html`. A test under the owning area exercises or imports `LAZY_GRAMMARS`.
- Source verification intent: The audit tool ships with the repository: node scripts/attribute-chunk-bytes.mjs (zero-dependency sourcemap VLQ byte attribution, aggregated by npm package / workspace directory). It verifies that vendor contains no workspace bytes, that the react family (including react/jsx-runtime) sits entirely in index, and that the npm side of index retains only the react family plus anser/clsx; the lazy grammar chunk count matches the LAZY_GRAMMARS table one to one; the browser keyless replay case is verbatim-identical to the pre-change baseline (apart from environment-specific local reds), so the two-chunk shell loads.

## How to read the implementation

1. Start with [`apps/web/vite.config.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/vite.config.ts) because it has the strongest evidence link to the note.
2. Read public types and entry points before implementation details. Write down the contract in plain language.
3. Follow the named constructs into their callers and tests. Confirm which component can actually enforce the rule.
4. Reconstruct one failure path: cancellation, crash, retry, stale state, partial write, or incompatible provider input—whichever the note is protecting against.
5. Only then map the contract into the current Rust repository. Target paths must come from that repository, never from guesswork in this wiki.

## Rust harness port

**Likely integration point:** read/write/patch filesystem capability.

Preserve the behavior and ownership rule. Do not mechanically translate Cordis, TypeScript declaration merging, Node streams, or package topology into Rust.

Relevant standing rules:

- **`rust/append-log`** — Keep durable facts append-only and derive views from them. Separate the synchronous fact boundary from asynchronous persistence.
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

- Tags: `class/architecture`, `concern/boundary`, `concern/evidence`, `concern/ownership`, `concern/performance`, `concern/recovery`, `domain/build-release`, `domain/configuration`, `domain/extensions`, `domain/filesystem`, `domain/jobs-tasks`, `domain/session-state`, `domain/shell-terminal`, `domain/testing`, `domain/ui-interaction`, `domain/web-retrieval`, `lifecycle/implemented`, `mechanism/event-sourcing`, `mechanism/projection`, `mechanism/streaming`
- Aliases: `VENDOR_PACKAGES`, `BOOT_GRAMMAR_FILES`, `FONT_EXTENSIONS`, `html`, `LAZY_GRAMMARS`, `vendor`, `dist/assets/`, `apps/web/vite.config.ts`, `manualChunks`, `@shikijs/langs`, `index.html`, `chunkFileNames`, `assetFileNames`, `assets/`
- Regex: `(?i)(VENDOR_PACKAGES|BOOT_GRAMMAR_FILES|FONT_EXTENSIONS|html|LAZY_GRAMMARS|vendor|dist/assets/|apps/web/vite\.config\.ts)`

```bash
rg -n --pcre2 "(?i)(VENDOR_PACKAGES|BOOT_GRAMMAR_FILES|FONT_EXTENSIONS|html|LAZY_GRAMMARS|vendor|dist/assets/|apps/web/vite\\.config\\.ts)" source-deepseek-harness
rg -l --fixed-strings "class/architecture" 03-agent-context
```

## Connected notes

- **`shares-code-with`** — [0314. Web GUI changes close the loop on the existing URL](0314-web-gui-changes-close-the-loop-on-the-existing-url.md): Shares source implementation: `apps/web`, `apps/web/package.json`.
- **`shares-code-with`** — [0065. Make packed chunk rows the default JSONL layout](0065-make-packed-chunk-rows-the-default-jsonl-layout.md): Shares source implementation: `apps/web`, `apps/web/package.json`.
- **`shares-code-with`** — [0500. Keyless browser e2e lane for the web GUI](0500-keyless-browser-e2e-lane-for-the-web-gui.md): Shares source implementation: `apps/web/package.json`.
- **`shares-code-with`** — [0183. Session List Browsing and Manual Workspace Order](0183-session-list-browsing-and-manual-workspace-order.md): Shares source implementation: `apps/web`, `apps/web/package.json`.
- **`shares-code-with`** — [0220. Web read card frontend --- the read tool's line window renders line-numbered and highlighted](0220-web-read-card-frontend-the-read-tool-s-line-window-renders-line-numbered.md): Shares source implementation: `packages/client/ui-primitives/src/markdown/highlight.ts`.
- **`shares-code-with`** — [0412. Web client syntax highlighting --- synchronous fine-grained shiki](0412-web-client-syntax-highlighting-synchronous-fine-grained-shiki.md): Shares source implementation: `packages/client/ui-primitives/src/markdown/CodeBlock.tsx`, `packages/client/ui-primitives/src/markdown/highlight.ts`.
- **`shares-code-with`** — [0596. `dsh migrate`/`dsh upgrade` seed the first turn with a skill](0596-dsh-migrate-dsh-upgrade-seed-the-first-turn-with-a-skill.md): Shares source implementation: `apps/web`, `apps/web/package.json`.
- **`shares-code-with`** — [0672. Replace tool-web's regex HTML-to-markdown converter with turndown](0672-replace-tool-web-s-regex-html-to-markdown-converter-with-turndown.md): Shares source implementation: `apps/web`, `apps/web/package.json`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0108-web-shell-dist-chunk-split-and-directory-layout.md`.
