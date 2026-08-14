---
id: "dsh-note-0256"
title: "Producer-declared context forms"
status: "implemented"
class: "feature"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/feature/2026-08-05-context-form-vocabulary.md"
implementation_evidence: "medium"
target_anchor: "read/write/patch filesystem capability"
tags:
  - "class/feature"
  - "concern/boundary"
  - "concern/compatibility"
  - "concern/discovery-routing"
  - "concern/ownership"
  - "concern/recovery"
  - "concern/schema-types"
  - "concern/simplification"
  - "domain/build-release"
  - "domain/context"
  - "domain/extensions"
  - "domain/filesystem"
  - "domain/llm"
  - "domain/multi-agent"
  - "domain/observability"
  - "domain/session-state"
  - "domain/shell-terminal"
  - "domain/storage"
  - "domain/testing"
  - "domain/ui-interaction"
  - "domain/web-retrieval"
  - "lifecycle/implemented"
  - "mechanism/policy"
  - "mechanism/projection"
  - "mechanism/registry"
aliases:
  - "plugin"
  - "form"
  - "instructions"
  - "source"
  - "renderContextSnapshot"
  - "renderContextSections"
  - "ToolCallView"
  - "events"
  - "snapshot"
  - "ContextFormed"
  - "MessageSource"
  - "notice"
  - "description"
  - "summary"
search_regex: "(?i)(plugin|form|instructions|source|renderContextSnapshot|renderContextSections|ToolCallView|events)"
---

# 0256. Producer-declared context forms — implementation context

## Open this when

Every logged non-user user/message rendered through one body: the whole message serialized as inline JSON. A reader opening a row met { "content": [ { "type": "text", "text": "…\n\n…" } ], "source": { … } }, where the escaping had collapsed the only thing worth reading --- the model-facing prose --- into a single line, and the producer fields sat inside the same blob. Naming the producer in the header (the source and steer marks decision) fixed who added this. It could not fix what kind of thing was added, because nothing in the log said so.

## Source decision

MessageSource gains an optional producer-declared form: ContextForm --- a small tagged vocabulary of information shapes, independent of kind: kind answers who produced this and carries no presentation choice. form answers what shape of information it is. Several producers may share one form, and one producer may emit more than one over a session. The vocabulary is semantic, never visual. A value states that the content is a file's instructions or a catalog of available items; colors, icons, ordering, and collapse defaults are the consumer's business and must not enter the union.

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/feature/2026-08-05-context-form-vocabulary.md](../02-notes/implemented/feature/2026-08-05-context-form-vocabulary.md)
- Pinned source: [.agents/notes/implemented/feature/2026-08-05-context-form-vocabulary.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/feature/2026-08-05-context-form-vocabulary.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`packages/goal/tool-goal/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/goal/tool-goal/src/index.ts) | package entry point | Core file in the package named by the note: `packages/goal/tool-goal`. | `named-package-member` |
| [`packages/jobs/tool-jobs/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/jobs/tool-jobs/src/index.ts) | package entry point | Core file in the package named by the note: `packages/jobs/tool-jobs`. Defines `snapshot`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/plan/plan-mode/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/plan/plan-mode/src/index.ts) | package entry point | Core file in the package named by the note: `packages/plan/plan-mode`. | `named-package-member` |
| [`packages/plan/plan-mode/src/types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/plan/plan-mode/src/types.ts) | public types and contract | Core file in the package named by the note: `packages/plan/plan-mode`. | `named-package-member` |
| [`packages/skill/tool-skill/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/skill/tool-skill/src/index.ts) | package entry point | Entry point or contract under the directory named by the note: `packages/skill/tool-skill`. Core file in the package named by the note: `packages/skill/tool-skill`. | `named-directory-member, named-package-member, symbol-definition` |
| [`packages/goal/tool-goal/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/goal/tool-goal/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/goal/tool-goal`. | `named-package-member` |
| [`packages/goal/tool-goal/src/authority.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/goal/tool-goal/src/authority.ts) | runtime implementation | Core file in the package named by the note: `packages/goal/tool-goal`. Defines `events`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/jobs/tool-jobs/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/jobs/tool-jobs/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/jobs/tool-jobs`. | `named-package-member` |
| [`packages/plan/plan-mode/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/plan/plan-mode/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/plan/plan-mode`. | `named-package-member` |
| [`packages/skill/tool-skill/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/skill/tool-skill/src/invariant.ts) | runtime contract checks | Entry point or contract under the directory named by the note: `packages/skill/tool-skill`. Core file in the package named by the note: `packages/skill/tool-skill`. | `named-directory-member, named-package-member` |
| [`packages/context/time-context/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/context/time-context/src/index.ts) | package entry point | Core file in the package named by the note: `packages/context/time-context`. | `named-package-member` |
| [`packages/context/tmux-context/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/context/tmux-context/src/index.ts) | package entry point | Core file in the package named by the note: `packages/context/tmux-context`. | `named-package-member` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `plugin` | `const` | [`apps/cli/src/args.ts:171`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/cli/src/args.ts#L171) | `const plugin = program.command('plugin').description('manage a profile\'s plugins by forwarding the remaining arguments to pnpm in the profile directory')` |
| `form` | `const` | [`packages/client/runtime/src/client/sessions/context-provenance.ts:115`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/runtime/src/client/sessions/context-provenance.ts#L115) | `const form = record === null ? null : readString(record, 'form')` |
| `instructions` | `const` | [`packages/context/agent-instructions/src/index.ts:137`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/context/agent-instructions/src/index.ts#L137) | `const instructions = await loadBaselineInstructionSet({` |
| `source` | `const` | [`packages/context/session-reference/src/index.ts:200`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/context/session-reference/src/index.ts#L200) | `const source: SessionReferenceSource = {` |
| `source` | `const` | [`packages/context/time-context/src/invariant.ts:106`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/context/time-context/src/invariant.ts#L106) | `const source = event.data.source` |
| `source` | `const` | [`packages/context/time-context/src/request-zone.ts:16`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/context/time-context/src/request-zone.ts#L16) | `const source = message.source` |
| `renderContextSnapshot` | `function` | [`packages/core/system-prompt/src/index.ts:224`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/system-prompt/src/index.ts#L224) | `export function renderContextSnapshot(assembly: PromptAssembly): string {` |
| `renderContextSections` | `function` | [`packages/core/system-prompt/src/index.ts:251`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/system-prompt/src/index.ts#L251) | `export function renderContextSections(assembly: PromptAssembly): ContextSnapshotSection[] {` |
| `ToolCallView` | `type` | [`packages/core/tools/src/presentation.ts:46`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/presentation.ts#L46) | `export type ToolCallView = GenericCallView \| TerminalCallView \| DiffCallView` |
| `events` | `const` | [`packages/goal/tool-goal/src/authority.ts:31`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/goal/tool-goal/src/authority.ts#L31) | `const events = agent.session.events` |
| `snapshot` | `const` | [`packages/jobs/tool-jobs/src/index.ts:394`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/jobs/tool-jobs/src/index.ts#L394) | `const snapshot = publicJob(ctx.jobs.get(id, exec.agent))` |
| `ContextFormed` | `type` | [`packages/llm/llm/src/message.ts:79`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/src/message.ts#L79) | `export type ContextFormed =` |
| `MessageSource` | `type` | [`packages/llm/llm/src/message.ts:126`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/src/message.ts#L126) | `export type MessageSource = MessageSourceMap[keyof MessageSourceMap]` |
| `notice` | `const` | [`packages/lsp/tool-lsp/src/render.ts:125`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/lsp/tool-lsp/src/render.ts#L125) | `const notice = \`\n… ${label} truncated (limit ${maxChars} characters).\`` |
| `description` | `const` | [`packages/skill/skill/src/index.ts:751`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/skill/skill/src/index.ts#L751) | `const description = skill.description` |
| `summary` | `const` | [`packages/skill/tool-skill/src/index.ts:134`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/skill/tool-skill/src/index.ts#L134) | `const summary = (await ctx.skills.list(lookup)).find(skill => skill.name === args.name)` |

### Tests and executable evidence

- [`packages/goal/tool-goal/tests/tool-goal.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/goal/tool-goal/tests/tool-goal.spec.ts) — A test under the owning area exercises or imports `MessageSource`. A test under the owning area exercises or imports `tool-goal`.
- [`packages/jobs/tool-jobs/tests/tool-jobs.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/jobs/tool-jobs/tests/tool-jobs.spec.ts) — A test under the owning area exercises or imports `notice`. A test under the owning area exercises or imports `tool-jobs`.
- [`packages/plan/plan-mode/tests/plan-mode.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/plan/plan-mode/tests/plan-mode.spec.ts) — A test under the owning area exercises or imports `presentCall`.
- [`packages/core/system-prompt/tests/scoped.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/system-prompt/tests/scoped.spec.ts) — A test under the owning area exercises or imports `renderContextSnapshot`.
- [`packages/skill/tool-skill/tests/tool-skill.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/skill/tool-skill/tests/tool-skill.spec.ts) — A test under the owning area exercises or imports `dsh-tool-skill`. A test under the owning area exercises or imports `skill-catalog`.
- [`packages/context/time-context/tests/invariant.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/context/time-context/tests/invariant.spec.ts) — A test under the owning area exercises or imports `time-context`.
- [`packages/workflow/tool-ralph/tests/tool-ralph.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/workflow/tool-ralph/tests/tool-ralph.spec.ts) — A test under the owning area exercises or imports `presentCall`.
- [`packages/context/time-context/tests/time-context.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/context/time-context/tests/time-context.e2e.ts) — A test under the owning area exercises or imports `time-context`.
- Source verification intent: packages/client/runtime pins the form projection, including the unknown, empty, wrongly-typed, and absent values that must degrade to opaque. packages/client/ui-conversation pins each body: the opaque body's preserved line breaks and source fields, the instructions body's file list and verbatim framing, the catalog body's entry list, and a catalog with unusable entries falling back to opaque. packages/skill/tool-skill pins the new source on first publication and replacement, republish behavior driven by the durable entries, and a malformed durable catalog leaving step observation intact.

## How to read the implementation

1. Start with [`packages/goal/tool-goal/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/goal/tool-goal/src/index.ts) because it has the strongest evidence link to the note.
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

- Tags: `class/feature`, `concern/boundary`, `concern/compatibility`, `concern/discovery-routing`, `concern/ownership`, `concern/recovery`, `concern/schema-types`, `concern/simplification`, `domain/build-release`, `domain/context`, `domain/extensions`, `domain/filesystem`, `domain/llm`, `domain/multi-agent`, `domain/observability`, `domain/session-state`, `domain/shell-terminal`, `domain/storage`, `domain/testing`, `domain/ui-interaction`, `domain/web-retrieval`, `lifecycle/implemented`, `mechanism/policy`, `mechanism/projection`, `mechanism/registry`
- Aliases: `plugin`, `form`, `instructions`, `source`, `renderContextSnapshot`, `renderContextSections`, `ToolCallView`, `events`, `snapshot`, `ContextFormed`, `MessageSource`, `notice`, `description`, `summary`
- Regex: `(?i)(plugin|form|instructions|source|renderContextSnapshot|renderContextSections|ToolCallView|events)`

```bash
rg -n --pcre2 "(?i)(plugin|form|instructions|source|renderContextSnapshot|renderContextSections|ToolCallView|events)" source-deepseek-harness
rg -l --fixed-strings "class/feature" 03-agent-context
```

## Connected notes

- **`source-link`** — [0252. Web transcript marks context source, recall, and steering](0252-web-transcript-marks-context-source-recall-and-steering.md): The source note links to this decision directly.
- **`shares-code-with`** — [0074. dsh source launch through the tsx ESM hook](0074-dsh-source-launch-through-the-tsx-esm-hook.md): Shares source implementation: `packages/jobs/tool-jobs/src/index.ts`, `packages/jobs/tool-jobs/src/invariant.ts`.
- **`shares-code-with`** — [0458. Plan-specific collaboration state](0458-plan-specific-collaboration-state.md): Shares source implementation: `packages/plan/plan-mode/src/index.ts`, `packages/plan/plan-mode/src/invariant.ts`.
- **`shares-code-with`** — [0161. Model-facing same-session goal tools](0161-model-facing-same-session-goal-tools.md): Shares source implementation: `packages/goal/tool-goal/src/authority.ts`, `packages/goal/tool-goal/src/index.ts`.
- **`shares-code-with`** — [0214. Plan review as a decision, not a question](0214-plan-review-as-a-decision-not-a-question.md): Shares source implementation: `packages/plan/plan-mode/src/index.ts`, `packages/plan/plan-mode/src/invariant.ts`.
- **`shares-code-with`** — [0157. Harness-level goal-based execution](0157-harness-level-goal-based-execution.md): Shares source implementation: `packages/goal/tool-goal/src/index.ts`, `packages/goal/tool-goal/src/invariant.ts`.
- **`shares-code-with`** — [0192. Skill catalog hot refresh](0192-skill-catalog-hot-refresh.md): Shares source implementation: `packages/skill/tool-skill/src/index.ts`, `packages/skill/tool-skill/src/invariant.ts`.
- **`shares-code-with`** — [0112. Per-preset standing mounts over a scope parent chain](0112-per-preset-standing-mounts-over-a-scope-parent-chain.md): Shares source implementation: `packages/jobs/tool-jobs/src/index.ts`, `packages/plan/plan-mode/src/index.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0256-producer-declared-context-forms.md`.
