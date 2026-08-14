---
id: "dsh-note-0484"
title: "One editor family in general-purpose presets"
status: "implemented"
class: "simplification"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/simplification/2026-08-10-default-presets-single-editor.md"
implementation_evidence: "medium"
target_anchor: "read/write/patch filesystem capability"
tags:
  - "class/simplification"
  - "concern/boundary"
  - "concern/discovery-routing"
  - "concern/evidence"
  - "concern/ownership"
  - "concern/schema-types"
  - "concern/simplification"
  - "domain/configuration"
  - "domain/extensions"
  - "domain/filesystem"
  - "domain/llm"
  - "domain/shell-terminal"
  - "domain/tools"
  - "lifecycle/implemented"
  - "mechanism/registry"
aliases:
  - "code"
  - "standard"
  - "str_replace_editor"
  - "dsh-tool-fs"
  - "dsh-tool-fs-search"
  - "dsh-tool-str-replace-editor"
  - "One editor family in general-purpose presets"
  - "simplification"
  - "boundary"
  - "discovery routing"
  - "evidence"
  - "ownership"
  - "schema types"
  - "configuration"
search_regex: "(?i)(code|standard|str_replace_editor|dsh\\-tool\\-fs|dsh\\-tool\\-fs\\-search|dsh\\-tool\\-str\\-replace\\-editor|One[- ]editor[- ]family[- ]in[- ]general\\-purpose[- ]presets|simplification)"
---

# 0484. One editor family in general-purpose presets — implementation context

## Open this when

The standard, code, and cordis presets exposed both the read/write/edit filesystem tools and str_replace_editor. The two interfaces overlap for ordinary file inspection and editing, so every request carried an additional tool schema without adding a distinct default capability. The minimal preset has a different composition contract: its exact two-tool roster intentionally includes str_replace_editor beside persistent bash.

## Source decision

The standard, code, and cordis preset configurations mount dsh-tool-fs and dsh-tool-fs-search, but do not mount dsh-tool-str-replace-editor. Code Mode therefore omits str_replace_editor from both its registry and generated SDK. The minimal preset continues to mount dsh-tool-str-replace-editor, and deployments or user-authored presets may still mount the plugin explicitly. This decision narrows the preset roster rather than removing the tool package or its Python runtime support.

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/simplification/2026-08-10-default-presets-single-editor.md](../02-notes/implemented/simplification/2026-08-10-default-presets-single-editor.md)
- Pinned source: [.agents/notes/implemented/simplification/2026-08-10-default-presets-single-editor.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/simplification/2026-08-10-default-presets-single-editor.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`vendor/cordis/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/cordis/src/index.ts) | package entry point | Core file in the package named by the note: `vendor/cordis`. | `named-package-member` |
| [`packages/fs/tool-fs/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/tool-fs/src/index.ts) | package entry point | Core file in the package named by the note: `packages/fs/tool-fs`. | `named-package-member` |
| [`packages/fs/tool-fs/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/tool-fs/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/fs/tool-fs`. | `named-package-member` |
| [`packages/fs/tool-fs-search/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/tool-fs-search/src/index.ts) | package entry point | Core file in the package named by the note: `packages/fs/tool-fs-search`. | `named-package-member` |
| [`packages/fs/tool-fs-search/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/tool-fs-search/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/fs/tool-fs-search`. | `named-package-member` |
| [`packages/fs/tool-str-replace-editor/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/tool-str-replace-editor/src/index.ts) | package entry point | Core file in the package named by the note: `packages/fs/tool-str-replace-editor`. | `named-package-member` |
| [`packages/fs/tool-str-replace-editor/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/tool-str-replace-editor/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/fs/tool-str-replace-editor`. | `named-package-member` |
| [`vendor/cordis`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/vendor/cordis) | package or module directory | The note names this package or capability. | `named-package` |
| [`packages/fs/tool-fs`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/packages/fs/tool-fs) | package or module directory | The note names this package or capability. | `named-package` |
| [`packages/fs/tool-fs-search`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/packages/fs/tool-fs-search) | package or module directory | The note names this package or capability. | `named-package` |
| [`packages/fs/tool-str-replace-editor`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/packages/fs/tool-str-replace-editor) | package or module directory | The note names this package or capability. | `named-package` |
| [`apps/cli/src/plugin.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/cli/src/plugin.ts) | runtime implementation | Defines `code`, a construct named by the note. | `symbol-definition` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `code` | `const` | [`apps/cli/src/plugin.ts:135`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/cli/src/plugin.ts#L135) | `const code = (result.error as NodeJS.ErrnoException).code` |
| `standard` | `let` | [`packages/client/web-react/src/scoped-slots.tsx:364`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/web-react/src/scoped-slots.tsx#L364) | `let standard = byInfo.get(info)` |

### Tests and executable evidence

- [`packages/typert/generator/tests/fixtures/type-model/packages/write/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/typert/generator/tests/fixtures/type-model/packages/write/src/index.ts) — Core file in the package named by the note: `packages/typert/generator/tests/fixtures/type-model/packages/write`.
- [`packages/typert/generator/tests/fixtures/type-model/packages/write`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/typert/generator/tests/fixtures/type-model/packages/write) — The note names this package or capability.
- [`packages/typert/generator/tests/fixtures/type-model/packages/write/package.json`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/typert/generator/tests/fixtures/type-model/packages/write/package.json) — Core file in the package named by the note: `packages/typert/generator/tests/fixtures/type-model/packages/write`.
- [`packages/fs/tool-fs-search/tests/tools.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/tool-fs-search/tests/tools.spec.ts) — A test under the owning area exercises or imports `dsh-tool-fs-search`.
- [`packages/fs/tool-fs-search/tests/rg-path.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/tool-fs-search/tests/rg-path.spec.ts) — A test under the owning area exercises or imports `dsh-tool-fs-search`.
- [`packages/fs/tool-fs-search/tests/load-path.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/tool-fs-search/tests/load-path.spec.ts) — A test under the owning area exercises or imports `dsh-tool-fs-search`.
- [`packages/fs/tool-fs-search/tests/integration.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/tool-fs-search/tests/integration.spec.ts) — A test under the owning area exercises or imports `dsh-tool-fs-search`.
- [`packages/fs/tool-str-replace-editor/tests/tools.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/tool-str-replace-editor/tests/tools.spec.ts) — A test under the owning area exercises or imports `str_replace_editor`. A test under the owning area exercises or imports `dsh-tool-str-replace-editor`.

## How to read the implementation

1. Start with [`vendor/cordis/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/cordis/src/index.ts) because it has the strongest evidence link to the note.
2. Read public types and entry points before implementation details. Write down the contract in plain language.
3. Follow the named constructs into their callers and tests. Confirm which component can actually enforce the rule.
4. Reconstruct one failure path: cancellation, crash, retry, stale state, partial write, or incompatible provider input—whichever the note is protecting against.
5. Only then map the contract into the current Rust repository. Target paths must come from that repository, never from guesswork in this wiki.

## Rust harness port

**Likely integration point:** read/write/patch filesystem capability.

Preserve the behavior and ownership rule. Do not mechanically translate Cordis, TypeScript declaration merging, Node streams, or package topology into Rust.

Relevant standing rules:

- **`rust/capability-security`** — Make granted capabilities explicit. On the Windows no-admin path, probe support at runtime and fail closed rather than silently running unsandboxed.
- **`rust/four-tool-surface`** — Preserve the four model tools: read, write, patch, and exec. Reuse internal capabilities without automatically exposing new model-facing tools.
- **`rust/newtype-ids`** — Represent identity domains with Rust newtypes; do not pass bare strings across session, call, turn, step, and agent boundaries.
- **`rust/provider-adapter`** — Keep provider wire quirks inside adapters; the loop and durable event vocabulary remain provider-neutral.
- **`rust/quickjs-boundary`** — QuickJS holds orchestration variables; Rust/Tokio owns async execution and exposes only explicitly granted capabilities.
- **`rust/serde-validation`** — Parse untrusted input into strict serde types and validate semantic constraints before executing side effects.
- **`rust/tagged-enums`** — Use serde-tagged enums for a closed wire vocabulary. Add an explicit extension envelope only where runtime extension is truly required.
- **`rust/tokio-cancellation`** — Thread one cancellation token through async work, close owned resources before reuse, and test cancellation at every await boundary.
- **`rust/trait-seam`** — Use a small trait at the capability boundary; keep policy and model-facing schemas outside the backend implementation.

### Smallest safe implementation slice

1. Express the semantic contract as Rust types and one owning boundary.
2. Add the smallest adapter or service needed by the existing loop; do not add a new model-facing tool unless the four-tool surface cannot express the job.
3. Record durable evidence before deriving summaries or UI state.
4. Add one happy-path test, one interruption test, and one replay/restart test where state survives a turn.
5. Keep the patch reversible until the source note's strongest failure case is proven in the Rust harness.

## Search handles

- Tags: `class/simplification`, `concern/boundary`, `concern/discovery-routing`, `concern/evidence`, `concern/ownership`, `concern/schema-types`, `concern/simplification`, `domain/configuration`, `domain/extensions`, `domain/filesystem`, `domain/llm`, `domain/shell-terminal`, `domain/tools`, `lifecycle/implemented`, `mechanism/registry`
- Aliases: `code`, `standard`, `str_replace_editor`, `dsh-tool-fs`, `dsh-tool-fs-search`, `dsh-tool-str-replace-editor`, `One editor family in general-purpose presets`, `simplification`, `boundary`, `discovery routing`, `evidence`, `ownership`, `schema types`, `configuration`
- Regex: `(?i)(code|standard|str_replace_editor|dsh\-tool\-fs|dsh\-tool\-fs\-search|dsh\-tool\-str\-replace\-editor|One[- ]editor[- ]family[- ]in[- ]general\-purpose[- ]presets|simplification)`

```bash
rg -n --pcre2 "(?i)(code|standard|str_replace_editor|dsh\\-tool\\-fs|dsh\\-tool\\-fs\\-search|dsh\\-tool\\-str\\-replace\\-editor|One[- ]editor[- ]family[- ]in[- ]general\\-purpose[- ]presets|simplification)" source-deepseek-harness
rg -l --fixed-strings "class/simplification" 03-agent-context
```

## Connected notes

- **`source-link`** — [0229. Even out the shipped tool rosters](0229-even-out-the-shipped-tool-rosters.md): The source note links to this decision directly.
- **`shares-code-with`** — [0284. A minimal read_image tool over existing seams](0284-a-minimal-read-image-tool-over-existing-seams.md): Shares source implementation: `packages/fs/tool-fs`, `packages/fs/tool-fs/src/index.ts`.
- **`shares-code-with`** — [0478. Copy-only preset authoring, and the way into a preset's files](0478-copy-only-preset-authoring-and-the-way-into-a-preset-s-files.md): Shares source implementation: `packages/typert/generator/tests/fixtures/type-model/packages/write/src/index.ts`, `vendor/cordis`.
- **`shares-code-with`** — [0548. Result-time applied-hunk diffs for file mutations](0548-result-time-applied-hunk-diffs-for-file-mutations.md): Shares source implementation: `packages/fs/tool-fs`, `packages/fs/tool-fs/src/index.ts`.
- **`shares-code-with`** — [0626. TUI diff context lines stay neutral](0626-tui-diff-context-lines-stay-neutral.md): Shares source implementation: `packages/fs/tool-fs`, `packages/fs/tool-fs/src/index.ts`.
- **`shares-code-with`** — [0321. Bound overwrite contextual-diff bases at the provider](0321-bound-overwrite-contextual-diff-bases-at-the-provider.md): Shares source implementation: `packages/fs/tool-fs`, `packages/fs/tool-fs/src/index.ts`.
- **`shares-code-with`** — [0210. Persistent Bash and string-replacement editor tools](0210-persistent-bash-and-string-replacement-editor-tools.md): Shares source implementation: `packages/fs/tool-str-replace-editor/src/index.ts`, `packages/fs/tool-str-replace-editor/src/invariant.ts`.
- **`shares-code-with`** — [0010. Filesystem capability seam --- ctx.fs, local backend, and model-facing filesystem tools](0010-filesystem-capability-seam-ctx-fs-local-backend-and-model-facing-filesys.md): Shares source implementation: `packages/fs/tool-fs/src/index.ts`, `vendor/cordis/src/index.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0484-one-editor-family-in-general-purpose-presets.md`.
