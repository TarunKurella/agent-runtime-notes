---
id: "dsh-note-0221"
title: "Read card --- the read tool's structured line window reaches the client"
status: "implemented"
class: "feature"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/feature/2026-07-30-web-read-card.md"
implementation_evidence: "high"
target_anchor: "read/write/patch filesystem capability"
tags:
  - "class/feature"
  - "concern/boundary"
  - "concern/discovery-routing"
  - "concern/evidence"
  - "concern/ownership"
  - "concern/recovery"
  - "concern/schema-types"
  - "domain/build-release"
  - "domain/configuration"
  - "domain/context"
  - "domain/extensions"
  - "domain/filesystem"
  - "domain/llm"
  - "domain/protocols"
  - "domain/session-state"
  - "domain/shell-terminal"
  - "domain/storage"
  - "domain/testing"
  - "domain/tools"
  - "domain/ui-interaction"
  - "lifecycle/implemented"
  - "mechanism/event-sourcing"
  - "mechanism/policy"
  - "mechanism/projection"
  - "mechanism/registry"
aliases:
  - "card"
  - "path"
  - "ToolCallView"
  - "GenericCallView"
  - "ToolResultView"
  - "GenericResultView"
  - "ReadResultView"
  - "LANG_BY_EXTENSION"
  - "langFromPath"
  - "readMetaFromMeta"
  - "offset"
  - "lang"
  - "totalLines"
  - "presentCall"
search_regex: "(?i)(card|path|ToolCallView|GenericCallView|ToolResultView|GenericResultView|ReadResultView|LANG_BY_EXTENSION)"
---

# 0221. Read card --- the read tool's structured line window reaches the client — implementation context

## Open this when

The read tool returns a canonical output object { path, offset, lines: [{ number, text }], totalLines }, but its presentation collapsed that structure. presentCall declared a GenericCallView (kind: 'read', a follow-along location) and presentResult returned a GenericResultView whose only content was the model-facing text with its …file… envelope stripped. A UI receiving that view saw one flattened text block: the line numbers were baked into the text as N: prefixes, the file's language was unknown, and totalLines was gone.

## Source decision

Add a fourth card tag, read, to the render-intent union --- result-side only. ToolResultView gains ReadResultView { card: 'read'; title?; path; lines: ReadFileLine[]; totalLines; lang?; content? }; ReadFileLine { number; text } is the shared line unit. ToolCallView is untouched: the pending state stays a GenericCallView (kind: 'read') because a call carries no file content until execute returns, so there is nothing structured to show at call time.

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/feature/2026-07-30-web-read-card.md](../02-notes/implemented/feature/2026-07-30-web-read-card.md)
- Pinned source: [.agents/notes/implemented/feature/2026-07-30-web-read-card.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/feature/2026-07-30-web-read-card.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`AGENTS.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/AGENTS.md) | package contract and examples | The source note names this file directly. | `named-file` |
| [`packages/context/agent-instructions/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/context/agent-instructions/src/index.ts) | package entry point | Core file in the package named by the note: `packages/context/agent-instructions`. | `named-package-member` |
| [`packages/context/agent-instructions/src/files.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/context/agent-instructions/src/files.ts) | runtime implementation | Core file in the package named by the note: `packages/context/agent-instructions`. Defines `path`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/context/agent-instructions/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/context/agent-instructions/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/context/agent-instructions`. | `named-package-member` |
| [`packages/context/agent-instructions`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/packages/context/agent-instructions) | package or module directory | The note names this package or capability. | `named-package` |
| [`vendor/cordis/src/fiber.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/cordis/src/fiber.ts) | runtime implementation | Defines `meta`, a construct named by the note. | `symbol-definition` |
| [`vendor/cordis/src/utils.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/cordis/src/utils.ts) | runtime implementation | Defines `lines`, a construct named by the note. | `symbol-definition` |
| [`packages/fs/tool-fs/src/read.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/tool-fs/src/read.ts) | runtime implementation | Defines `offset`, a construct named by the note. Defines `lang`, a construct named by the note. | `symbol-definition` |
| [`packages/fs/tool-fs/src/read-render.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/tool-fs/src/read-render.ts) | runtime implementation | Defines `readMetaFromMeta`, a construct named by the note. Defines `langFromPath`, a construct named by the note. | `symbol-definition` |
| [`packages/core/tools/src/presentation.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/presentation.ts) | runtime implementation | Defines `GenericCallView`, a construct named by the note. Defines `GenericResultView`, a construct named by the note. | `symbol-definition` |
| [`packages/workflow/tool-ralph/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/workflow/tool-ralph/src/index.ts) | package entry point | Defines `presentCall`, a construct named by the note. Defines `presentResult`, a construct named by the note. | `symbol-definition` |
| [`packages/terminal/terminal-bash/src/session.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/terminal/terminal-bash/src/session.ts) | runtime implementation | Defines `totalLines`, a construct named by the note. | `symbol-definition` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `card` | `const` | [`packages/client/ui-primitives/src/HoverCard.tsx:140`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-primitives/src/HoverCard.tsx#L140) | `const card = cardRef.current` |
| `path` | `const` | [`packages/context/agent-instructions/src/files.ts:248`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/context/agent-instructions/src/files.ts#L248) | `const path = join(dir, candidate)` |
| `ToolCallView` | `type` | [`packages/core/tools/src/presentation.ts:46`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/presentation.ts#L46) | `export type ToolCallView = GenericCallView \| TerminalCallView \| DiffCallView` |
| `GenericCallView` | `interface` | [`packages/core/tools/src/presentation.ts:53`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/presentation.ts#L53) | `export interface GenericCallView {` |
| `ToolResultView` | `type` | [`packages/core/tools/src/presentation.ts:140`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/presentation.ts#L140) | `export type ToolResultView = GenericResultView \| TerminalResultView \| DiffResultView \| SearchResultView \| ReadResultView \| WebResultView` |
| `GenericResultView` | `interface` | [`packages/core/tools/src/presentation.ts:146`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/presentation.ts#L146) | `export interface GenericResultView {` |
| `ReadResultView` | `interface` | [`packages/core/tools/src/presentation.ts:281`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/presentation.ts#L281) | `export interface ReadResultView {` |
| `LANG_BY_EXTENSION` | `const` | [`packages/fs/tool-fs/src/read-render.ts:178`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/tool-fs/src/read-render.ts#L178) | `const LANG_BY_EXTENSION: Readonly<Record<string, string>> = {` |
| `langFromPath` | `function` | [`packages/fs/tool-fs/src/read-render.ts:199`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/tool-fs/src/read-render.ts#L199) | `export function langFromPath(path: string): string \| undefined {` |
| `readMetaFromMeta` | `function` | [`packages/fs/tool-fs/src/read-render.ts:258`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/tool-fs/src/read-render.ts#L258) | `export function readMetaFromMeta(meta: unknown): FsReadMeta \| undefined {` |
| `offset` | `const` | [`packages/fs/tool-fs/src/read.ts:58`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/tool-fs/src/read.ts#L58) | `const offset = args.offset === undefined ? 1 : parsePositiveInteger(args.offset, 'offset')` |
| `lang` | `const` | [`packages/fs/tool-fs/src/read.ts:124`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/tool-fs/src/read.ts#L124) | `const lang = langFromPath(value.path)` |
| `totalLines` | `const` | [`packages/terminal/terminal-bash/src/session.ts:320`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/terminal/terminal-bash/src/session.ts#L320) | `const totalLines = snapshot.text.length === 0 ? 0 : lines.length` |
| `presentCall` | `function` | [`packages/workflow/tool-ralph/src/index.ts:394`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/workflow/tool-ralph/src/index.ts#L394) | `function presentCall(args: RalphCallArgs): ToolCallView {` |
| `presentResult` | `function` | [`packages/workflow/tool-ralph/src/index.ts:398`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/workflow/tool-ralph/src/index.ts#L398) | `function presentResult(args: RalphCallArgs, result: { content: ContentBlock[]; isError: boolean }): ToolResultView {` |
| `meta` | `const` | [`vendor/cordis/src/fiber.ts:444`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/cordis/src/fiber.ts#L444) | `const meta: EffectMeta = { label, children: [] }` |

### Tests and executable evidence

- [`packages/fs/tool-fs/tests/tools.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/tool-fs/tests/tools.spec.ts) — The source note names this file directly. A test under the owning area exercises or imports `lang`.
- [`packages/fs/tool-fs/tests/read-render.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/tool-fs/tests/read-render.spec.ts) — The source note names this file directly. A test under the owning area exercises or imports `readMetaFromMeta`.
- [`packages/workflow/tool-ralph/tests/tool-ralph.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/workflow/tool-ralph/tests/tool-ralph.spec.ts) — A test under the owning area exercises or imports `presentCall`. A test under the owning area exercises or imports `presentResult`.
- [`packages/context/agent-instructions/tests/agent-instructions.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/context/agent-instructions/tests/agent-instructions.e2e.ts) — A test under the owning area exercises or imports `agent-instructions`.
- [`packages/context/agent-instructions/tests/agent-instructions.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/context/agent-instructions/tests/agent-instructions.spec.ts) — A test under the owning area exercises or imports `totalLines`. A test under the owning area exercises or imports `agent-instructions`.
- Source verification intent: packages/fs/tool-fs/tests/read-render.spec.ts unit-tests langFromPath (known extensions case-insensitively, extension read after the last segment and last dot, and the undefined cases: dotfile, extensionless, trailing dot, unknown) and readMetaFromMeta (a well-formed narrow with and without lang, and every rejection: non-object, array, missing or wrong-typed path/totalLines/lines, a malformed line entry, a non-string lang, and --- because the function narrows the opaque persisted meta boundary --- the semantically invalid paths a well-typed replayed JSON can still carry: an offset that is not a 1-based integer.

## How to read the implementation

1. Start with [`AGENTS.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/AGENTS.md) because it has the strongest evidence link to the note.
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
- **`rust/context-evidence`** — Compaction may shrink presentation, but it must not erase the evidence ledger, file activity, or durable session facts.
- **`rust/four-tool-surface`** — Preserve the four model tools: read, write, patch, and exec. Reuse internal capabilities without automatically exposing new model-facing tools.
- **`rust/newtype-ids`** — Represent identity domains with Rust newtypes; do not pass bare strings across session, call, turn, step, and agent boundaries.
- **`rust/process-discipline`** — Treat this as repository or release policy unless it changes a runtime contract. Do not add runtime machinery for a process-only lesson.
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

- Tags: `class/feature`, `concern/boundary`, `concern/discovery-routing`, `concern/evidence`, `concern/ownership`, `concern/recovery`, `concern/schema-types`, `domain/build-release`, `domain/configuration`, `domain/context`, `domain/extensions`, `domain/filesystem`, `domain/llm`, `domain/protocols`, `domain/session-state`, `domain/shell-terminal`, `domain/storage`, `domain/testing`, `domain/tools`, `domain/ui-interaction`, `lifecycle/implemented`, `mechanism/event-sourcing`, `mechanism/policy`, `mechanism/projection`, `mechanism/registry`
- Aliases: `card`, `path`, `ToolCallView`, `GenericCallView`, `ToolResultView`, `GenericResultView`, `ReadResultView`, `LANG_BY_EXTENSION`, `langFromPath`, `readMetaFromMeta`, `offset`, `lang`, `totalLines`, `presentCall`
- Regex: `(?i)(card|path|ToolCallView|GenericCallView|ToolResultView|GenericResultView|ReadResultView|LANG_BY_EXTENSION)`

```bash
rg -n --pcre2 "(?i)(card|path|ToolCallView|GenericCallView|ToolResultView|GenericResultView|ReadResultView|LANG_BY_EXTENSION)" source-deepseek-harness
rg -l --fixed-strings "class/feature" 03-agent-context
```

## Connected notes

- **`source-link`** — [0023. Tagged render-intent union for tool-call presentation](0023-tagged-render-intent-union-for-tool-call-presentation.md): The source note links to this decision directly.
- **`source-link`** — [0049. Canonical tool output contract](0049-canonical-tool-output-contract.md): The source note links to this decision directly.
- **`source-link`** — [0207. Web terminal card --- the bash render intent reaches the browser](0207-web-terminal-card-the-bash-render-intent-reaches-the-browser.md): The source note links to this decision directly.
- **`shares-code-with`** — [0136. Workspace context instruction files](0136-workspace-context-instruction-files.md): Shares source implementation: `packages/context/agent-instructions`, `packages/context/agent-instructions/src/files.ts`.
- **`shares-code-with`** — [0277. User-explicit skill invocation at the pre-step gesture boundary](0277-user-explicit-skill-invocation-at-the-pre-step-gesture-boundary.md): Shares source implementation: `packages/context/agent-instructions`, `packages/context/agent-instructions/src/index.ts`.
- **`shares-code-with`** — [0193. tmux-location context](0193-tmux-location-context.md): Shares source implementation: `packages/context/agent-instructions/src/index.ts`, `packages/context/agent-instructions/src/invariant.ts`.
- **`shares-code-with`** — [0252. Web transcript marks context source, recall, and steering](0252-web-transcript-marks-context-source-recall-and-steering.md): Shares source implementation: `packages/context/agent-instructions/src/index.ts`, `packages/context/agent-instructions/src/invariant.ts`.
- **`shares-code-with`** — [0564. Bash-backed grep and glob discovery tools](0564-bash-backed-grep-and-glob-discovery-tools.md): Shares source implementation: `packages/fs/tool-fs/src/read.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0221-read-card-the-read-tool-s-structured-line-window-reaches-the-client.md`.
