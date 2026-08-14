---
id: "dsh-note-0422"
title: "Native GitHub stacks and optional PR rebases"
status: "implemented"
class: "process"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/process/2026-08-02-native-github-stacks-and-optional-rebases.md"
implementation_evidence: "medium"
target_anchor: "repository tests and engineering policy"
tags:
  - "class/process"
  - "concern/boundary"
  - "concern/compatibility"
  - "concern/concurrency"
  - "concern/evidence"
  - "concern/human-control"
  - "concern/lifecycle"
  - "concern/ownership"
  - "concern/recovery"
  - "concern/schema-types"
  - "domain/build-release"
  - "domain/llm"
  - "domain/session-state"
  - "domain/testing"
  - "lifecycle/implemented"
  - "mechanism/policy"
aliases:
  - "stack"
  - "position"
  - "PullRequest.stack"
  - "stackEntry.position"
  - "gh stack merge <stack-number> --yes --merge"
  - "MERGED"
  - "--force"
  - "Native GitHub stacks and optional PR rebases"
  - "process"
  - "boundary"
  - "compatibility"
  - "concurrency"
  - "evidence"
  - "human control"
search_regex: "(?i)(stack|position|PullRequest\\.stack|stackEntry\\.position|gh[- ]stack[- ]merge[- ]<stack\\-number>[- ]\\-\\-yes[- ]\\-\\-merge|MERGED|\\-\\-force|Native[- ]GitHub[- ]stacks[- ]and[- ]optional[- ]PR[- ]rebases)"
---

# 0422. Native GitHub stacks and optional PR rebases — implementation context

## Open this when

A dependent PR chain represented only by base branches has no official stack identity. Landing it requires manually merging one PR at a time, preserving intermediate branches, retargeting every child, and reconstructing whether the chain survived. GitHub's native stacked-PR feature instead carries the order, applies trunk rules and CI to every layer, and owns bottom-up merges and retargeting. A blanket prohibition on rewriting reviewed branches also excludes the native gh stack synchronization workflow, whose cascading rebase updates each active layer and publishes it with lease protection.

## Source decision

Every same-repository chain of two or more dependent PRs uses GitHub's official stack object before landing. Live PullRequest.stack and stackEntry.position fields are authoritative. An unstacked chain whose PRs have one author is linked automatically in bottom-to-top order with gh stack link; mixed or unavailable authors require user confirmation. Missing native support and cross-fork chains hard-stop. Existing membership in conflicting stacks or an official order that disagrees with the branch topology requires user direction before any stack is dissolved or rebuilt.

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/process/2026-08-02-native-github-stacks-and-optional-rebases.md](../02-notes/implemented/process/2026-08-02-native-github-stacks-and-optional-rebases.md)
- Pinned source: [.agents/notes/implemented/process/2026-08-02-native-github-stacks-and-optional-rebases.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/process/2026-08-02-native-github-stacks-and-optional-rebases.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`.agents/skills/dsh-pre-push-checks/SKILL.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/skills/dsh-pre-push-checks/SKILL.md) | package contract and examples | The source note names this file directly. | `named-file` |
| [`.agents/skills/dsh-merging-stacked-prs/SKILL.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/skills/dsh-merging-stacked-prs/SKILL.md) | package contract and examples | The source note names this file directly. | `named-file` |
| [`docs/cookbook/responding-to-pr-review-on-a-stack.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/cookbook/responding-to-pr-review-on-a-stack.md) | package contract and examples | The source note names this file directly. | `named-file` |
| [`packages/boot/app-boot/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/boot/app-boot/src/index.ts) | package entry point | Defines `stack`, a construct named by the note. | `symbol-definition` |
| [`packages/lsp/lsp-stdio/src/translate.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/lsp/lsp-stdio/src/translate.ts) | runtime implementation | Defines `position`, a construct named by the note. | `symbol-definition` |
| [`packages/typert/generator/src/analyzer.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/typert/generator/src/analyzer.ts) | runtime implementation | Defines `position`, a construct named by the note. | `symbol-definition` |
| [`packages/subagent/subagent/src/list-children.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/subagent/subagent/src/list-children.ts) | runtime implementation | Defines `stack`, a construct named by the note. | `symbol-definition` |
| [`packages/session-query/session-query/src/tracing.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/session-query/session-query/src/tracing.ts) | runtime implementation | Defines `stack`, a construct named by the note. | `symbol-definition` |
| [`packages/extensions/cordis-host-runner/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/extensions/cordis-host-runner/src/index.ts) | package entry point | Defines `stack`, a construct named by the note. | `symbol-definition` |
| [`packages/workflow/workflow-worker-thread/src/realm.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/workflow/workflow-worker-thread/src/realm.ts) | runtime implementation | Defines `stack`, a construct named by the note. | `symbol-definition` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `stack` | `const` | [`packages/boot/app-boot/src/index.ts:799`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/boot/app-boot/src/index.ts#L799) | `const stack = deepest instanceof Error && deepest !== cause ? \`\n${deepest.stack ?? deepest.message}\` : ''` |
| `stack` | `const` | [`packages/extensions/cordis-host-runner/src/index.ts:1256`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/extensions/cordis-host-runner/src/index.ts#L1256) | `const stack = 'stack' in error && typeof error.stack === 'string' ? error.stack : undefined` |
| `position` | `const` | [`packages/lsp/lsp-stdio/src/translate.ts:130`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/lsp/lsp-stdio/src/translate.ts#L130) | `const position = value as Record<string, unknown>` |
| `stack` | `const` | [`packages/session-query/session-query/src/tracing.ts:225`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/session-query/session-query/src/tracing.ts#L225) | `const stack = [{ sessionId, descendants }]` |
| `stack` | `const` | [`packages/subagent/subagent/src/list-children.ts:313`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/subagent/subagent/src/list-children.ts#L313) | `const stack: PositionedCandidate[] = (children.get(rootSessionId) ?? [])` |
| `position` | `const` | [`packages/typert/generator/src/analyzer.ts:426`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/typert/generator/src/analyzer.ts#L426) | `const position = sourceFile.getLineAndCharacterOfPosition(statement.getStart(sourceFile))` |
| `position` | `const` | [`packages/typert/generator/src/analyzer.ts:2169`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/typert/generator/src/analyzer.ts#L2169) | `const position = annotationPosition(owner, purpose)` |
| `position` | `const` | [`packages/typert/generator/src/analyzer.ts:2508`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/typert/generator/src/analyzer.ts#L2508) | `const position = sourceFile.getLineAndCharacterOfPosition(node.getStart(sourceFile))` |
| `position` | `const` | [`packages/typert/generator/src/analyzer.ts:3070`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/typert/generator/src/analyzer.ts#L3070) | `const position = diagnostic.file.getLineAndCharacterOfPosition(diagnostic.start)` |
| `stack` | `const` | [`packages/workflow/workflow-worker-thread/src/realm.ts:30`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/workflow/workflow-worker-thread/src/realm.ts#L30) | `const stack = (error as { stack?: unknown } \| null \| undefined)?.stack` |

### Tests and executable evidence

- No test file was tied to this note with enough confidence. Read the note's verification section and search the owning package's `tests/` directory.
- Source verification intent: The stack landing skill verifies native support, same-repository branches, live authors, official membership and order, merge range, and final merged state. The stack review guide keeps fixes on their introducing layer and covers both propagation histories. The pre-push workflow owns lease protection and immediate post-sync evidence.

## How to read the implementation

1. Start with [`.agents/skills/dsh-pre-push-checks/SKILL.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/skills/dsh-pre-push-checks/SKILL.md) because it has the strongest evidence link to the note.
2. Read public types and entry points before implementation details. Write down the contract in plain language.
3. Follow the named constructs into their callers and tests. Confirm which component can actually enforce the rule.
4. Reconstruct one failure path: cancellation, crash, retry, stale state, partial write, or incompatible provider input—whichever the note is protecting against.
5. Only then map the contract into the current Rust repository. Target paths must come from that repository, never from guesswork in this wiki.

## Rust harness port

**Likely integration point:** repository tests and engineering policy.

Preserve the behavior and ownership rule. Do not mechanically translate Cordis, TypeScript declaration merging, Node streams, or package topology into Rust.

Relevant standing rules:

- **`rust/append-log`** — Keep durable facts append-only and derive views from them. Separate the synchronous fact boundary from asynchronous persistence.
- **`rust/four-tool-surface`** — Preserve the four model tools: read, write, patch, and exec. Reuse internal capabilities without automatically exposing new model-facing tools.
- **`rust/newtype-ids`** — Represent identity domains with Rust newtypes; do not pass bare strings across session, call, turn, step, and agent boundaries.
- **`rust/process-discipline`** — Treat this as repository or release policy unless it changes a runtime contract. Do not add runtime machinery for a process-only lesson.
- **`rust/provider-adapter`** — Keep provider wire quirks inside adapters; the loop and durable event vocabulary remain provider-neutral.
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

- Tags: `class/process`, `concern/boundary`, `concern/compatibility`, `concern/concurrency`, `concern/evidence`, `concern/human-control`, `concern/lifecycle`, `concern/ownership`, `concern/recovery`, `concern/schema-types`, `domain/build-release`, `domain/llm`, `domain/session-state`, `domain/testing`, `lifecycle/implemented`, `mechanism/policy`
- Aliases: `stack`, `position`, `PullRequest.stack`, `stackEntry.position`, `gh stack merge <stack-number> --yes --merge`, `MERGED`, `--force`, `Native GitHub stacks and optional PR rebases`, `process`, `boundary`, `compatibility`, `concurrency`, `evidence`, `human control`
- Regex: `(?i)(stack|position|PullRequest\.stack|stackEntry\.position|gh[- ]stack[- ]merge[- ]<stack\-number>[- ]\-\-yes[- ]\-\-merge|MERGED|\-\-force|Native[- ]GitHub[- ]stacks[- ]and[- ]optional[- ]PR[- ]rebases)`

```bash
rg -n --pcre2 "(?i)(stack|position|PullRequest\\.stack|stackEntry\\.position|gh[- ]stack[- ]merge[- ]<stack\\-number>[- ]\\-\\-yes[- ]\\-\\-merge|MERGED|\\-\\-force|Native[- ]GitHub[- ]stacks[- ]and[- ]optional[- ]PR[- ]rebases)" source-deepseek-harness
rg -l --fixed-strings "class/process" 03-agent-context
```

## Connected notes

- **`source-link`** — [0410. Retarget PR bases incrementally](0410-retarget-pr-bases-incrementally.md): The source note links to this decision directly.
- **`shares-code-with`** — [0106. Subagent list identity via the projection unit](0106-subagent-list-identity-via-the-projection-unit.md): Shares source implementation: `packages/subagent/subagent/src/list-children.ts`.
- **`shares-code-with`** — [0263. The continuable child return channel is an obligation](0263-the-continuable-child-return-channel-is-an-obligation.md): Shares source implementation: `packages/boot/app-boot/src/index.ts`.
- **`shares-code-with`** — [0637. Parallel GitHub CI gates](0637-parallel-github-ci-gates.md): Shares source implementation: `packages/typert/generator/src/analyzer.ts`.
- **`shares-code-with`** — [0375. Bound cold blank-session verification](0375-bound-cold-blank-session-verification.md): Shares source implementation: `packages/boot/app-boot/src/index.ts`.
- **`shares-code-with`** — [0513. Record last activity in the session index](0513-record-last-activity-in-the-session-index.md): Shares source implementation: `packages/boot/app-boot/src/index.ts`.
- **`shares-code-with`** — [0329. fail-loud releases the terminal before exiting](0329-fail-loud-releases-the-terminal-before-exiting.md): Shares source implementation: `packages/boot/app-boot/src/index.ts`.
- **`shares-code-with`** — [0152. SQLite FTS5 session search](0152-sqlite-fts5-session-search.md): Shares source implementation: `packages/session-query/session-query/src/tracing.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0422-native-github-stacks-and-optional-pr-rebases.md`.
