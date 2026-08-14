---
id: "dsh-note-0192"
title: "Skill catalog hot refresh"
status: "implemented"
class: "feature"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/feature/2026-07-27-skill-catalog-hot-refresh.md"
implementation_evidence: "medium"
target_anchor: "read/write/patch filesystem capability"
tags:
  - "class/feature"
  - "concern/boundary"
  - "concern/compatibility"
  - "concern/concurrency"
  - "concern/discovery-routing"
  - "concern/evidence"
  - "concern/ownership"
  - "concern/performance"
  - "concern/recovery"
  - "concern/simplification"
  - "domain/agent-loop"
  - "domain/build-release"
  - "domain/configuration"
  - "domain/context"
  - "domain/extensions"
  - "domain/filesystem"
  - "domain/llm"
  - "domain/observability"
  - "domain/session-state"
  - "domain/shell-terminal"
  - "domain/storage"
  - "domain/testing"
  - "domain/tools"
  - "domain/ui-interaction"
  - "lifecycle/implemented"
  - "mechanism/event-sourcing"
  - "mechanism/generation"
  - "mechanism/registry"
aliases:
  - "invalidate"
  - "UserMessage"
  - "description"
  - "skill"
  - "snapshot"
  - ".agents/skills"
  - "ctx.skills.snapshot"
  - "ctx.skills.registerProvider"
  - "@deepseek-ai/dsh-skill-filesystem"
  - "SKILL.md"
  - "fs.watchFile"
  - "watchFile"
  - "unlinkDir"
  - "@deepseek-ai/dsh-tool-skill"
search_regex: "(?i)(invalidate|UserMessage|description|skill|snapshot|\\.agents/skills|ctx\\.skills\\.snapshot|ctx\\.skills\\.registerProvider)"
---

# 0192. Skill catalog hot refresh — implementation context

## Open this when

Skill summaries are model routing input, but local skills can appear, disappear, or be renamed after a session starts. IDEs, Git operations, shell commands, and other processes can all mutate .agents/skills without going through the harness filesystem tools. A startup-only catalog leaves the model unaware of new skills and able to call deleted names. Treating every instruction-body edit as a catalog revision would instead couple progressive loading to unnecessary prompt churn. Filesystem updates are also non-atomic from the observer's perspective.

## Source decision

The skill capability separates catalog membership from instruction-body loading. ctx.skills.snapshot() returns summaries plus a completeness bit. ctx.skills.registerProvider(factory) gives the synchronous factory one registration-scoped { signal, invalidate } control: invalidate() dirties only that exact active registration and discards completed catalog caches, while the signal aborts when registration fails or is disposed.

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/feature/2026-07-27-skill-catalog-hot-refresh.md](../02-notes/implemented/feature/2026-07-27-skill-catalog-hot-refresh.md)
- Pinned source: [.agents/notes/implemented/feature/2026-07-27-skill-catalog-hot-refresh.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/feature/2026-07-27-skill-catalog-hot-refresh.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`packages/skill/skill/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/skill/skill/src/index.ts) | package entry point | Core file in the package named by the note: `packages/skill/skill`. Defines `description`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/skill/skill/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/skill/skill/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/skill/skill`. | `named-package-member` |
| [`packages/skill/tool-skill/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/skill/tool-skill/src/index.ts) | package entry point | Core file in the package named by the note: `packages/skill/tool-skill`. Defines `skill`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/skill/tool-skill/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/skill/tool-skill/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/skill/tool-skill`. | `named-package-member` |
| [`packages/skill/skill-filesystem/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/skill/skill-filesystem/src/index.ts) | package entry point | Core file in the package named by the note: `packages/skill/skill-filesystem`. Defines `description`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/skill/skill-filesystem/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/skill/skill-filesystem/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/skill/skill-filesystem`. | `named-package-member` |
| [`.agents/skills`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/.agents/skills) | package or module directory | The source note names this implementation area directly. | `named-directory` |
| [`packages/skill/skill`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/packages/skill/skill) | package or module directory | The note names this package or capability. | `named-package` |
| [`packages/skill/tool-skill`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/packages/skill/tool-skill) | package or module directory | The note names this package or capability. | `named-package` |
| [`packages/skill/skill-filesystem`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/packages/skill/skill-filesystem) | package or module directory | The note names this package or capability. | `named-package` |
| [`packages/llm/llm/src/message.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/src/message.ts) | runtime implementation | Defines `UserMessage`, a construct named by the note. | `symbol-definition` |
| [`packages/client/ui-skill/src/client/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-skill/src/client/index.ts) | package entry point | Defines `invalidate`, a construct named by the note. | `symbol-definition` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `invalidate` | `const` | [`packages/client/ui-skill/src/client/index.ts:117`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-skill/src/client/index.ts#L117) | `const invalidate = (key: SessionId): void => {` |
| `UserMessage` | `interface` | [`packages/llm/llm/src/message.ts:141`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/src/message.ts#L141) | `export interface UserMessage extends Message {` |
| `description` | `const` | [`packages/skill/skill-filesystem/src/index.ts:811`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/skill/skill-filesystem/src/index.ts#L811) | `const description = stringField(parsed.data, 'description')` |
| `skill` | `const` | [`packages/skill/skill/src/index.ts:574`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/skill/skill/src/index.ts#L574) | `const skill = entry.candidate` |
| `description` | `const` | [`packages/skill/skill/src/index.ts:751`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/skill/skill/src/index.ts#L751) | `const description = skill.description` |
| `skill` | `const` | [`packages/skill/tool-skill/src/index.ts:141`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/skill/tool-skill/src/index.ts#L141) | `const skill = await ctx.skills.get(args.name, lookup)` |
| `skill` | `const` | [`packages/skill/tool-skill/src/index.ts:189`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/skill/tool-skill/src/index.ts#L189) | `const skill = await ctx.skills.get(name, lookup)` |
| `snapshot` | `const` | [`packages/skill/tool-skill/src/index.ts:221`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/skill/tool-skill/src/index.ts#L221) | `const snapshot = toolVisible` |

### Tests and executable evidence

- [`packages/typert/generator/tests/fixtures/type-model/packages/write/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/typert/generator/tests/fixtures/type-model/packages/write/src/index.ts) — Core file in the package named by the note: `packages/typert/generator/tests/fixtures/type-model/packages/write`.
- [`packages/typert/generator/tests/fixtures/type-model/packages/write`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/typert/generator/tests/fixtures/type-model/packages/write) — The note names this package or capability.
- [`packages/typert/generator/tests/fixtures/type-model/packages/write/package.json`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/typert/generator/tests/fixtures/type-model/packages/write/package.json) — Core file in the package named by the note: `packages/typert/generator/tests/fixtures/type-model/packages/write`.
- [`packages/skill/skill-filesystem/tests/skill-filesystem-watcher.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/skill/skill-filesystem/tests/skill-filesystem-watcher.spec.ts) — A test under the owning area exercises or imports `watchFile`. A test under the owning area exercises or imports `unlinkDir`.
- [`apps/cli/tests/fixtures/dsh-badge/snapshot.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/cli/tests/fixtures/dsh-badge/snapshot.ts) — Contains the exact code literal `agent/pre-step` named by the note.
- [`packages/client/runtime/tests/wire-events.client.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/runtime/tests/wire-events.client.spec.ts) — Contains the exact code literal `skills/change` named by the note.
- Source verification intent: Registry tests pin registration-scoped invalidation, revocation, signal abort, contained observer failures, incomplete candidates, bounded generation retries, and stale-name rejection. Local-provider tests cover bundle and flat-file creation, removal, rename, root creation/deletion/recreation including an unobserved root unlinkDir, description changes, body-only edits, first-party observation, symlinks, polling options, persistent watcher failures with loadable candidates, event coalescing, bounded projects, teardown, and transient reads.

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

- **`rust/append-log`** — Keep durable facts append-only and derive views from them. Separate the synchronous fact boundary from asynchronous persistence.
- **`rust/capability-security`** — Make granted capabilities explicit. On the Windows no-admin path, probe support at runtime and fail closed rather than silently running unsandboxed.
- **`rust/context-evidence`** — Compaction may shrink presentation, but it must not erase the evidence ledger, file activity, or durable session facts.
- **`rust/four-tool-surface`** — Preserve the four model tools: read, write, patch, and exec. Reuse internal capabilities without automatically exposing new model-facing tools.
- **`rust/process-discipline`** — Treat this as repository or release policy unless it changes a runtime contract. Do not add runtime machinery for a process-only lesson.
- **`rust/provider-adapter`** — Keep provider wire quirks inside adapters; the loop and durable event vocabulary remain provider-neutral.
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

- Tags: `class/feature`, `concern/boundary`, `concern/compatibility`, `concern/concurrency`, `concern/discovery-routing`, `concern/evidence`, `concern/ownership`, `concern/performance`, `concern/recovery`, `concern/simplification`, `domain/agent-loop`, `domain/build-release`, `domain/configuration`, `domain/context`, `domain/extensions`, `domain/filesystem`, `domain/llm`, `domain/observability`, `domain/session-state`, `domain/shell-terminal`, `domain/storage`, `domain/testing`, `domain/tools`, `domain/ui-interaction`, `lifecycle/implemented`, `mechanism/event-sourcing`, `mechanism/generation`, `mechanism/registry`
- Aliases: `invalidate`, `UserMessage`, `description`, `skill`, `snapshot`, `.agents/skills`, `ctx.skills.snapshot`, `ctx.skills.registerProvider`, `@deepseek-ai/dsh-skill-filesystem`, `SKILL.md`, `fs.watchFile`, `watchFile`, `unlinkDir`, `@deepseek-ai/dsh-tool-skill`
- Regex: `(?i)(invalidate|UserMessage|description|skill|snapshot|\.agents/skills|ctx\.skills\.snapshot|ctx\.skills\.registerProvider)`

```bash
rg -n --pcre2 "(?i)(invalidate|UserMessage|description|skill|snapshot|\\.agents/skills|ctx\\.skills\\.snapshot|ctx\\.skills\\.registerProvider)" source-deepseek-harness
rg -l --fixed-strings "class/feature" 03-agent-context
```

## Connected notes

- **`shares-code-with`** — [0277. User-explicit skill invocation at the pre-step gesture boundary](0277-user-explicit-skill-invocation-at-the-pre-step-gesture-boundary.md): Shares source implementation: `packages/skill/skill`, `packages/skill/skill/src/index.ts`.
- **`shares-code-with`** — [0116. The skill registry is host-held and layered per scope](0116-the-skill-registry-is-host-held-and-layered-per-scope.md): Shares source implementation: `packages/skill/skill-filesystem/src/index.ts`, `packages/skill/skill-filesystem/src/invariant.ts`.
- **`shares-code-with`** — [0580. TUI skill slash command](0580-tui-skill-slash-command.md): Shares source implementation: `packages/skill/skill`, `packages/skill/skill/src/index.ts`.
- **`shares-code-with`** — [0262. Bundled dsh badge skill](0262-bundled-dsh-badge-skill.md): Shares source implementation: `packages/skill/skill-filesystem/src/index.ts`, `packages/skill/skill-filesystem/src/invariant.ts`.
- **`shares-code-with`** — [0204. Independent model and user skill invocation policy](0204-independent-model-and-user-skill-invocation-policy.md): Shares source implementation: `packages/skill/skill/src/index.ts`, `packages/skill/tool-skill`.
- **`shares-code-with`** — [0143. Skill system --- progressive disclosure instructions for agents](0143-skill-system-progressive-disclosure-instructions-for-agents.md): Shares source implementation: `packages/skill/skill/src/index.ts`, `packages/skill/skill/src/invariant.ts`.
- **`shares-code-with`** — [0049. Canonical tool output contract](0049-canonical-tool-output-contract.md): Shares source implementation: `packages/skill/skill/src/index.ts`, `packages/skill/skill/src/invariant.ts`.
- **`shares-code-with`** — [0252. Web transcript marks context source, recall, and steering](0252-web-transcript-marks-context-source-recall-and-steering.md): Shares source implementation: `packages/skill/tool-skill/src/index.ts`, `packages/skill/tool-skill/src/invariant.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0192-skill-catalog-hot-refresh.md`.
