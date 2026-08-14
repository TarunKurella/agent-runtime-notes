---
id: "dsh-note-0619"
title: "Tool-card single-row fields render inline"
status: "archived"
class: "bug-fix"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/archived/bug-fix/2026-07-27-tool-card-single-row-fields-inline.md"
implementation_evidence: "medium"
target_anchor: "read/write/patch filesystem capability"
tags:
  - "class/bug-fix"
  - "concern/boundary"
  - "concern/human-control"
  - "domain/filesystem"
  - "domain/llm"
  - "domain/shell-terminal"
  - "domain/ui-interaction"
  - "lifecycle/archived"
aliases:
  - "displayText"
  - "split"
  - "description"
  - "cwd"
  - "contentText"
  - "displayInlineText"
  - "S=/tmp\\x0aecho …"
  - "multilineTerminal"
  - "tui.spec.ts"
  - "Tool-card single-row fields render inline"
  - "bug fix"
  - "boundary"
  - "human control"
  - "filesystem"
search_regex: "(?i)(displayText|split|description|contentText|displayInlineText|S=/tmp\\\\x0aecho[- ]…|multilineTerminal|tui\\.spec\\.ts)"
---

# 0619. Tool-card single-row fields render inline — implementation context

## Open this when

A tool card's title, description, cwd, and pending $ echo are each one logical row. The bash tool sets the card title (and description) directly from the model's command and description, which for a multi-line bash script contain real newlines. These fields were escaped with displayText, which deliberately preserves \n as structural layout. A multi-line title therefore broke onto extra terminal rows that the card's line accounting did not reserve, so the title's later lines overwrote the description, the output, or the editor's steering hint --- the card rendered as garbled, overlapping text.

## Source decision

Single-row card fields use displayInlineText (which escapes \n to the literal \x0a) instead of displayText: the card title, the terminal-card description and cwd meta rows, and the pending $ echo. Each stays on exactly one row, so a multi-line command can no longer break rows and collide with adjacent lines. Genuinely multi-line fields --- captured command output and the contentText result body --- keep displayText plus split('\n'), because those legitimately occupy multiple rows.

## Decision status

Historical design. Use it to understand an older pressure or failure; locate the replacing decision before implementing anything.

- Raw note: [archived/bug-fix/2026-07-27-tool-card-single-row-fields-inline.md](../02-notes/archived/bug-fix/2026-07-27-tool-card-single-row-fields-inline.md)
- Pinned source: [.agents/notes/archived/bug-fix/2026-07-27-tool-card-single-row-fields-inline.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/archived/bug-fix/2026-07-27-tool-card-single-row-fields-inline.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`packages/skill/skill/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/skill/skill/src/index.ts) | package entry point | Defines `description`, a construct named by the note. | `symbol-definition` |
| [`packages/core/tools/src/py-types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/py-types.ts) | runtime implementation | Defines `description`, a construct named by the note. | `symbol-definition` |
| [`packages/fs/tool-fs/src/session-cwd.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/tool-fs/src/session-cwd.ts) | runtime implementation | Defines `cwd`, a construct named by the note. | `symbol-definition` |
| [`packages/host/apiproxy/src/api-proxy.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/src/api-proxy.ts) | runtime implementation | Defines `cwd`, a construct named by the note. | `symbol-definition` |
| [`packages/typert/generator/src/analyzer.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/typert/generator/src/analyzer.ts) | runtime implementation | Defines `description`, a construct named by the note. | `symbol-definition` |
| [`packages/fs/tool-fs-search/src/search-core.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/tool-fs-search/src/search-core.ts) | runtime implementation | Defines `cwd`, a construct named by the note. | `symbol-definition` |
| [`packages/session-query/session-query/src/extraction.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/session-query/session-query/src/extraction.ts) | runtime implementation | Defines `contentText`, a construct named by the note. | `symbol-definition` |
| [`packages/client/ui-trajectory/src/client/TrajectoryTable.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-trajectory/src/client/TrajectoryTable.tsx) | runtime implementation | Defines `displayText`, a construct named by the note. Defines `split`, a construct named by the note. | `symbol-definition` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `displayText` | `const` | [`packages/client/ui-trajectory/src/client/TrajectoryTable.tsx:982`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-trajectory/src/client/TrajectoryTable.tsx#L982) | `const displayText = useMemo(` |
| `split` | `const` | [`packages/client/ui-trajectory/src/client/TrajectoryTable.tsx:2554`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-trajectory/src/client/TrajectoryTable.tsx#L2554) | `const split = details.parentElement` |
| `split` | `const` | [`packages/client/ui-trajectory/src/client/TrajectoryTable.tsx:2594`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-trajectory/src/client/TrajectoryTable.tsx#L2594) | `const split = details.parentElement` |
| `description` | `const` | [`packages/core/tools/src/py-types.ts:224`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/py-types.ts#L224) | `const description = (schema as Record<string, unknown>).description` |
| `description` | `const` | [`packages/core/tools/src/py-types.ts:572`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/py-types.ts#L572) | `const description = describe(fieldSchema)` |
| `description` | `const` | [`packages/core/tools/src/py-types.ts:804`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/py-types.ts#L804) | `const description = describe(schema)` |
| `cwd` | `const` | [`packages/fs/tool-fs-search/src/search-core.ts:223`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/tool-fs-search/src/search-core.ts#L223) | `const cwd = exec.agent?.session.header.cwd` |
| `cwd` | `const` | [`packages/fs/tool-fs/src/session-cwd.ts:24`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/tool-fs/src/session-cwd.ts#L24) | `const cwd = exec.agent?.session.header.cwd` |
| `cwd` | `const` | [`packages/fs/tool-fs/src/session-cwd.ts:41`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/tool-fs/src/session-cwd.ts#L41) | `const cwd = policyWorkspaceRoot ?? sessionCwd(exec, requestedPath)` |
| `cwd` | `const` | [`packages/host/apiproxy/src/api-proxy.ts:2180`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/src/api-proxy.ts#L2180) | `const cwd = workspace?.path ?? request.payload.cwd ?? defaults.cwd` |
| `cwd` | `const` | [`packages/host/apiproxy/src/api-proxy.ts:3224`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/src/api-proxy.ts#L3224) | `const cwd = session.header.cwd` |
| `contentText` | `function` | [`packages/session-query/session-query/src/extraction.ts:64`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/session-query/session-query/src/extraction.ts#L64) | `function contentText(content: readonly SessionContentBlock[]): string {` |
| `description` | `const` | [`packages/skill/skill/src/index.ts:751`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/skill/skill/src/index.ts#L751) | `const description = skill.description` |
| `description` | `const` | [`packages/typert/generator/src/analyzer.ts:2815`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/typert/generator/src/analyzer.ts#L2815) | `const description = normalizedDocText(ts.getTextOfJSDocComment(block.comment))` |

### Tests and executable evidence

- No test file was tied to this note with enough confidence. Read the note's verification section and search the owning package's `tests/` directory.

## How to read the implementation

1. Start with [`packages/skill/skill/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/skill/skill/src/index.ts) because it has the strongest evidence link to the note.
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
- **`rust/provider-adapter`** — Keep provider wire quirks inside adapters; the loop and durable event vocabulary remain provider-neutral.
- **`rust/tokio-cancellation`** — Thread one cancellation token through async work, close owned resources before reuse, and test cancellation at every await boundary.
- **`rust/trait-seam`** — Use a small trait at the capability boundary; keep policy and model-facing schemas outside the backend implementation.
- **Status gate** — Historical design. Use it to understand an older pressure or failure; locate the replacing decision before implementing anything.

### Smallest safe implementation slice

1. Express the semantic contract as Rust types and one owning boundary.
2. Add the smallest adapter or service needed by the existing loop; do not add a new model-facing tool unless the four-tool surface cannot express the job.
3. Record durable evidence before deriving summaries or UI state.
4. Add one happy-path test, one interruption test, and one replay/restart test where state survives a turn.
5. Keep the patch reversible until the source note's strongest failure case is proven in the Rust harness.

## Search handles

- Tags: `class/bug-fix`, `concern/boundary`, `concern/human-control`, `domain/filesystem`, `domain/llm`, `domain/shell-terminal`, `domain/ui-interaction`, `lifecycle/archived`
- Aliases: `displayText`, `split`, `description`, `cwd`, `contentText`, `displayInlineText`, `S=/tmp\x0aecho …`, `multilineTerminal`, `tui.spec.ts`, `Tool-card single-row fields render inline`, `bug fix`, `boundary`, `human control`, `filesystem`
- Regex: `(?i)(displayText|split|description|contentText|displayInlineText|S=/tmp\\x0aecho[- ]…|multilineTerminal|tui\.spec\.ts)`

```bash
rg -n --pcre2 "(?i)(displayText|split|description|contentText|displayInlineText|S=/tmp\\\\x0aecho[- ]\u2026|multilineTerminal|tui\\.spec\\.ts)" source-deepseek-harness
rg -l --fixed-strings "class/bug-fix" 03-agent-context
```

## Connected notes

- **`source-link`** — [0673. Copyable TUI transcript without gutter bars](0673-copyable-tui-transcript-without-gutter-bars.md): The source note links to this decision directly.
- **`shares-code-with`** — [0622. Web conversation UI polish sweep](0622-web-conversation-ui-polish-sweep.md): Shares source implementation: `packages/core/tools/src/py-types.ts`, `packages/fs/tool-fs-search/src/search-core.ts`.
- **`shares-code-with`** — [0366. Preset cards clamp their description instead of sizing the roster](0366-preset-cards-clamp-their-description-instead-of-sizing-the-roster.md): Shares source implementation: `packages/core/tools/src/py-types.ts`, `packages/host/apiproxy/src/api-proxy.ts`.
- **`shares-code-with`** — [0345. Workspace New Session reuse hijacked cwd-matching unaccounted blank sessions](0345-workspace-new-session-reuse-hijacked-cwd-matching-unaccounted-blank-sess.md): Shares source implementation: `packages/fs/tool-fs-search/src/search-core.ts`, `packages/fs/tool-fs/src/session-cwd.ts`.
- **`shares-code-with`** — [0642. installer skips the clone when run from inside a checkout](0642-installer-skips-the-clone-when-run-from-inside-a-checkout.md): Shares source implementation: `packages/fs/tool-fs-search/src/search-core.ts`, `packages/fs/tool-fs/src/session-cwd.ts`.
- **`shares-code-with`** — [0130. Multiplex concurrent ACP sessions over one connection](0130-multiplex-concurrent-acp-sessions-over-one-connection.md): Shares source implementation: `packages/fs/tool-fs/src/session-cwd.ts`, `packages/host/apiproxy/src/api-proxy.ts`.
- **`shares-code-with`** — [0432. Unified GitHub label taxonomy](0432-unified-github-label-taxonomy.md): Shares source implementation: `packages/core/tools/src/py-types.ts`, `packages/skill/skill/src/index.ts`.
- **`shares-code-with`** — [0237. opening a produced file from the web UI](0237-opening-a-produced-file-from-the-web-ui.md): Shares source implementation: `packages/core/tools/src/py-types.ts`, `packages/host/apiproxy/src/api-proxy.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0619-tool-card-single-row-fields-render-inline.md`.
