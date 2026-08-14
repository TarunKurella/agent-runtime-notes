---
id: "dsh-note-0672"
title: "Replace tool-web's regex HTML-to-markdown converter with turndown"
status: "archived"
class: "simplification"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/archived/simplification/2026-07-26-turndown-for-tool-web-html-markdown.md"
implementation_evidence: "high"
target_anchor: "sandbox capability boundary"
tags:
  - "class/simplification"
  - "concern/boundary"
  - "concern/evidence"
  - "concern/ownership"
  - "concern/recovery"
  - "concern/schema-types"
  - "concern/simplification"
  - "domain/build-release"
  - "domain/configuration"
  - "domain/extensions"
  - "domain/llm"
  - "domain/protocols"
  - "domain/security"
  - "domain/session-state"
  - "domain/storage"
  - "domain/testing"
  - "domain/tools"
  - "lifecycle/archived"
  - "mechanism/event-sourcing"
  - "mechanism/policy"
aliases:
  - "web"
  - "turndown"
  - "formatFetchOutput"
  - "dsh-tool-web"
  - "src/html.ts"
  - "<h1-6>"
  - "web_fetch"
  - "packages/web/tool-web/src/fetch.ts"
  - "headingStyle: 'atx"
  - "codeBlockStyle: 'fenced"
  - "bulletListMarker: '-"
  - "@joplin/turndown-plugin-gfm"
  - "fetchMaxOutputChars"
  - "html.ts"
search_regex: "(?i)(turndown|formatFetchOutput|dsh\\-tool\\-web|src/html\\.ts|<h1\\-6>|web_fetch|packages/web/tool\\-web/src/fetch\\.ts|headingStyle:[- ]'atx)"
---

# 0672. Replace tool-web's regex HTML-to-markdown converter with turndown — implementation context

## Open this when

dsh-tool-web's src/html.ts (~86 lines, ~40 lines of dedicated tests; deleted by this change) converted fetched HTML to markdown with regexes: strip script/style/noscript/comments, convert //, decode numeric entities plus a 12-entry named-entity table, collapse whitespace. The module's own JSDoc said "A richer converter can replace it without changing the seam or tool schema", and the README's Known Limitations documented it as "a minimal regex converter, not an HTML parser --- tables, images, and nested formatting are lost." The web capability seam note assigns HTML→markdown to this package as presentation, so.

## Source decision

packages/web/tool-web/src/fetch.ts owns a module-level turndown instance (headingStyle: 'atx', codeBlockStyle: 'fenced', bulletListMarker: '-' --- fixed model-facing presentation, not deployment tunables) with @joplin/turndown-plugin-gfm's composite gfm plugin for tables/strikethrough and remove(['script', 'style', 'noscript']) replacing the old wholesale drops. formatFetchOutput limits both the source prefix converted synchronously and the complete rendered output with fetchMaxOutputChars (default 200,000), so a custom provider cannot make conversion work unbounded before the output cap applies.

## Decision status

Historical design. Use it to understand an older pressure or failure; locate the replacing decision before implementing anything.

- Raw note: [archived/simplification/2026-07-26-turndown-for-tool-web-html-markdown.md](../02-notes/archived/simplification/2026-07-26-turndown-for-tool-web-html-markdown.md)
- Pinned source: [.agents/notes/archived/simplification/2026-07-26-turndown-for-tool-web-html-markdown.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/archived/simplification/2026-07-26-turndown-for-tool-web-html-markdown.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`examples/acp-agent/web.cordis.yml`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/examples/acp-agent/web.cordis.yml) | composition and configuration | The source note names this file directly. | `named-file` |
| [`packages/web/tool-web/src/fetch.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/web/tool-web/src/fetch.ts) | runtime implementation | The source note names this file directly. Core file in the package named by the note: `packages/web/tool-web`. | `named-file, named-package-member, symbol-definition` |
| [`packages/web/tool-web/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/web/tool-web/src/index.ts) | package entry point | Core file in the package named by the note: `packages/web/tool-web`. | `named-package-member` |
| [`packages/web/tool-web/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/web/tool-web/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/web/tool-web`. | `named-package-member` |
| [`apps/web`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/apps/web) | package or module directory | The note names this package or capability. | `named-package` |
| [`packages/web/tool-web`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/packages/web/tool-web) | package or module directory | The note names this package or capability. | `named-package` |
| [`apps/cli/src/args.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/cli/src/args.ts) | runtime implementation | Defines `web`, a construct named by the note. | `symbol-definition` |
| [`apps/web/package.json`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/package.json) | composition and configuration | Core file in the package named by the note: `apps/web`. | `named-package-member` |
| [`packages/web/tool-web/README.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/web/tool-web/README.md) | package contract and examples | Core file in the package named by the note: `packages/web/tool-web`. | `named-package-member` |
| [`packages/web/tool-web/package.json`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/web/tool-web/package.json) | composition and configuration | Core file in the package named by the note: `packages/web/tool-web`. | `named-package-member` |
| [`pnpm-lock.yaml`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/pnpm-lock.yaml) | composition and configuration | Contains the exact code literal `dsh-tool-web` named by the note. | `exact-code-occurrence` |
| [`docs/tool-catalog.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/tool-catalog.md) | package contract and examples | Contains the exact code literal `dsh-tool-web` named by the note. | `exact-code-occurrence` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `web` | `const` | [`apps/cli/src/args.ts:156`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/cli/src/args.ts#L156) | `const web = program.command('web').description('boot the web profile (alias of --profile web); the web app\'s own flags follow')` |
| `turndown` | `const` | [`packages/web/tool-web/src/fetch.ts:25`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/web/tool-web/src/fetch.ts#L25) | `const turndown = new TurndownService({` |
| `formatFetchOutput` | `function` | [`packages/web/tool-web/src/fetch.ts:328`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/web/tool-web/src/fetch.ts#L328) | `export function formatFetchOutput(result: WebFetchResult, maxOutputChars: number): string {` |

### Tests and executable evidence

- [`packages/web/tool-web/tests/tool-web.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/web/tool-web/tests/tool-web.spec.ts) — The source note names this file directly. A test under the owning area exercises or imports `dsh-tool-web`.
- [`apps/web/tests/smoke-real.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/smoke-real.e2e.ts) — A test under the owning area exercises or imports `web_fetch`.
- [`apps/web/tests/built-boot.snapshot.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/built-boot.snapshot.ts) — A test under the owning area exercises or imports `web_fetch`.
- [`apps/web/tests/web-search-round.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/web-search-round.e2e.ts) — A test under the owning area exercises or imports `dsh-tool-web`.
- [`packages/web/tool-web/tests/spill.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/web/tool-web/tests/spill.spec.ts) — A test under the owning area exercises or imports `dsh-tool-web`. A test under the owning area exercises or imports `web_fetch`.
- [`apps/web/tests/shipped-composition.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/shipped-composition.e2e.ts) — A test under the owning area exercises or imports `web_fetch`.
- [`packages/web/tool-web/tests/load-path.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/web/tool-web/tests/load-path.spec.ts) — A test under the owning area exercises or imports `dsh-tool-web`. A test under the owning area exercises or imports `web_fetch`.
- [`packages/web/tool-web/tests/integration.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/web/tool-web/tests/integration.spec.ts) — A test under the owning area exercises or imports `dsh-tool-web`. A test under the owning area exercises or imports `web_fetch`.
- Source verification intent: packages/web/tool-web/tests/tool-web.spec.ts covers the turndown conversion surface (entities, links, tables, nesting, script/style/noscript removal), ignored table spans, source-prefix and complete-output bounds, fast raw-HTML passthrough for deep or deceptively closed nesting, linear handling of malformed tags, the residual converter-throw fallback, and exact and tiny output budgets; per-file coverage on the package src is 100%. The web-fetch acp-agent snapshot pins the assembled behavior keylessly end to end (real Loader composition, real HTTP fetch, real conversion).

## How to read the implementation

1. Start with [`examples/acp-agent/web.cordis.yml`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/examples/acp-agent/web.cordis.yml) because it has the strongest evidence link to the note.
2. Read public types and entry points before implementation details. Write down the contract in plain language.
3. Follow the named constructs into their callers and tests. Confirm which component can actually enforce the rule.
4. Reconstruct one failure path: cancellation, crash, retry, stale state, partial write, or incompatible provider input—whichever the note is protecting against.
5. Only then map the contract into the current Rust repository. Target paths must come from that repository, never from guesswork in this wiki.

## Rust harness port

**Likely integration point:** sandbox capability boundary.

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
- **Status gate** — Historical design. Use it to understand an older pressure or failure; locate the replacing decision before implementing anything.

### Smallest safe implementation slice

1. Express the semantic contract as Rust types and one owning boundary.
2. Add the smallest adapter or service needed by the existing loop; do not add a new model-facing tool unless the four-tool surface cannot express the job.
3. Record durable evidence before deriving summaries or UI state.
4. Add one happy-path test, one interruption test, and one replay/restart test where state survives a turn.
5. Keep the patch reversible until the source note's strongest failure case is proven in the Rust harness.

## Search handles

- Tags: `class/simplification`, `concern/boundary`, `concern/evidence`, `concern/ownership`, `concern/recovery`, `concern/schema-types`, `concern/simplification`, `domain/build-release`, `domain/configuration`, `domain/extensions`, `domain/llm`, `domain/protocols`, `domain/security`, `domain/session-state`, `domain/storage`, `domain/testing`, `domain/tools`, `lifecycle/archived`, `mechanism/event-sourcing`, `mechanism/policy`
- Aliases: `web`, `turndown`, `formatFetchOutput`, `dsh-tool-web`, `src/html.ts`, `<h1-6>`, `web_fetch`, `packages/web/tool-web/src/fetch.ts`, `headingStyle: 'atx`, `codeBlockStyle: 'fenced`, `bulletListMarker: '-`, `@joplin/turndown-plugin-gfm`, `fetchMaxOutputChars`, `html.ts`
- Regex: `(?i)(turndown|formatFetchOutput|dsh\-tool\-web|src/html\.ts|<h1\-6>|web_fetch|packages/web/tool\-web/src/fetch\.ts|headingStyle:[- ]'atx)`

```bash
rg -n --pcre2 "(?i)(turndown|formatFetchOutput|dsh\\-tool\\-web|src/html\\.ts|<h1\\-6>|web_fetch|packages/web/tool\\-web/src/fetch\\.ts|headingStyle:[- ]'atx)" source-deepseek-harness
rg -l --fixed-strings "class/simplification" 03-agent-context
```

## Connected notes

- **`shares-code-with`** — [0663. Prune unused web seam fields](0663-prune-unused-web-seam-fields.md): Shares source implementation: `packages/web/tool-web`, `packages/web/tool-web/README.md`.
- **`shares-code-with`** — [0655. Drop the unconsumed web observation surface --- the `providers-change` event and the status methods](0655-drop-the-unconsumed-web-observation-surface-the-providers-change-event-a.md): Shares source implementation: `packages/web/tool-web`, `packages/web/tool-web/README.md`.
- **`shares-code-with`** — [0596. `dsh migrate`/`dsh upgrade` seed the first turn with a skill](0596-dsh-migrate-dsh-upgrade-seed-the-first-turn-with-a-skill.md): Shares source implementation: `apps/cli/src/args.ts`, `apps/web`.
- **`shares-code-with`** — [0032. Tool output spill policy](0032-tool-output-spill-policy.md): Shares source implementation: `packages/web/tool-web/src/fetch.ts`, `packages/web/tool-web/src/index.ts`.
- **`shares-code-with`** — [0024. Prompt variables and tool-guidance ownership](0024-prompt-variables-and-tool-guidance-ownership.md): Shares source implementation: `packages/web/tool-web/src/fetch.ts`, `packages/web/tool-web/src/index.ts`.
- **`shares-code-with`** — [0065. Make packed chunk rows the default JSONL layout](0065-make-packed-chunk-rows-the-default-jsonl-layout.md): Shares source implementation: `apps/web`, `apps/web/package.json`.
- **`shares-code-with`** — [0235. Default Web search in shipped compositions](0235-default-web-search-in-shipped-compositions.md): Shares source implementation: `packages/web/tool-web/src/index.ts`, `packages/web/tool-web/src/invariant.ts`.
- **`shares-code-with`** — [0597. `dsh meta` boots the TUI over the harness checkout](0597-dsh-meta-boots-the-tui-over-the-harness-checkout.md): Shares source implementation: `apps/cli/src/args.ts`, `apps/web`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0672-replace-tool-web-s-regex-html-to-markdown-converter-with-turndown.md`.
