---
id: "dsh-note-0136"
title: "Workspace context instruction files"
status: "implemented"
class: "feature"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/feature/2026-06-24-workspace-context.md"
implementation_evidence: "medium"
target_anchor: "sandbox capability boundary"
tags:
  - "class/feature"
  - "concern/boundary"
  - "concern/cancellation-timeout"
  - "concern/compatibility"
  - "concern/concurrency"
  - "concern/discovery-routing"
  - "concern/evidence"
  - "concern/lifecycle"
  - "concern/ownership"
  - "concern/performance"
  - "concern/recovery"
  - "concern/schema-types"
  - "concern/simplification"
  - "concern/trust"
  - "domain/agent-loop"
  - "domain/build-release"
  - "domain/configuration"
  - "domain/context"
  - "domain/extensions"
  - "domain/filesystem"
  - "domain/llm"
  - "domain/protocols"
  - "domain/security"
  - "domain/session-state"
  - "domain/shell-terminal"
  - "domain/storage"
  - "domain/testing"
  - "lifecycle/implemented"
  - "mechanism/event-sourcing"
  - "mechanism/projection"
aliases:
  - "cwd"
  - "digest"
  - "FsVersion"
  - "maxBytes"
  - "streamText"
  - "set"
  - "replace"
  - "AGENTS.md"
  - "CLAUDE.md"
  - "packages/context/agent-instructions"
  - "@deepseek-ai/dsh-agent-instructions"
  - "{ maxBytes } | false"
  - "agent/pre-step"
  - "tools/result"
search_regex: "(?i)(digest|FsVersion|maxBytes|streamText|replace|AGENTS\\.md|CLAUDE\\.md|packages/context/agent\\-instructions)"
---

# 0136. Workspace context instruction files — implementation context

## Open this when

Repository guidance such as AGENTS.md belongs in a coding session's effective context so project conventions, build commands, and review rules arrive without repeated user pasting. The stdio and ACP products need the same behavior, isolated by session cwd: a global system-prompt section leaks one workspace's files into another live ACP session. Neighboring products establish useful conventions but differ in details.

## Source decision

The implementation lives in packages/context/agent-instructions as @deepseek-ai/dsh-agent-instructions. It is a request-context extension, not a core service or a filesystem backend. The shared demo spine and Host Runtime mount it from an explicit { maxBytes } | false deployment choice; dsh web enables a 65,536-byte budget while the Host Runtime's headless consumer disables it. The plugin consumes agent/pre-step, immutable tools/result outcomes, session/event boundaries, and the optional ctx.fs capability. The plugin does not statically inject fs.

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/feature/2026-06-24-workspace-context.md](../02-notes/implemented/feature/2026-06-24-workspace-context.md)
- Pinned source: [.agents/notes/implemented/feature/2026-06-24-workspace-context.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/feature/2026-06-24-workspace-context.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`packages/context/agent-instructions/README.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/context/agent-instructions/README.md) | package contract and examples | Entry point or contract under the directory named by the note: `packages/context/agent-instructions`. The source note names this file directly. | `named-directory-member, named-file, named-package-member` |
| [`packages/util/home-paths/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/util/home-paths/src/index.ts) | package entry point | Core file in the package named by the note: `packages/util/home-paths`. | `named-package-member` |
| [`packages/util/home-paths/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/util/home-paths/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/util/home-paths`. | `named-package-member` |
| [`packages/context/agent-instructions/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/context/agent-instructions/src/index.ts) | package entry point | Entry point or contract under the directory named by the note: `packages/context/agent-instructions`. Core file in the package named by the note: `packages/context/agent-instructions`. | `named-directory-member, named-package-member, symbol-definition` |
| [`packages/context/agent-instructions/src/files.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/context/agent-instructions/src/files.ts) | runtime implementation | Entry point or contract under the directory named by the note: `packages/context/agent-instructions`. Core file in the package named by the note: `packages/context/agent-instructions`. | `named-directory-member, named-package-member, symbol-definition` |
| [`packages/context/agent-instructions/src/state.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/context/agent-instructions/src/state.ts) | runtime implementation | Entry point or contract under the directory named by the note: `packages/context/agent-instructions`. Core file in the package named by the note: `packages/context/agent-instructions`. | `named-directory-member, named-package-member, symbol-definition` |
| [`packages/context/agent-instructions/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/context/agent-instructions/src/invariant.ts) | runtime contract checks | Entry point or contract under the directory named by the note: `packages/context/agent-instructions`. Core file in the package named by the note: `packages/context/agent-instructions`. | `named-directory-member, named-package-member` |
| [`packages/context/agent-instructions/package.json`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/context/agent-instructions/package.json) | composition and configuration | Entry point or contract under the directory named by the note: `packages/context/agent-instructions`. Core file in the package named by the note: `packages/context/agent-instructions`. | `named-directory-member, named-package-member` |
| [`packages/context/agent-instructions`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/packages/context/agent-instructions) | package or module directory | The source note names this implementation area directly. The note names this package or capability. | `named-directory, named-package` |
| [`packages/util/home-paths`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/packages/util/home-paths) | package or module directory | The note names this package or capability. | `named-package` |
| [`packages/fs/fs/src/types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/fs/src/types.ts) | public types and contract | Defines `FsVersion`, a construct named by the note. | `symbol-definition` |
| [`vendor/loader/src/config/entry.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/loader/src/config/entry.ts) | runtime implementation | Defines `replace`, a construct named by the note. | `symbol-definition` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `cwd` | `const` | [`packages/context/agent-instructions/src/files.ts:298`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/context/agent-instructions/src/files.ts#L298) | `const cwd = resolve(options.cwd)` |
| `digest` | `const` | [`packages/context/agent-instructions/src/files.ts:378`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/context/agent-instructions/src/files.ts#L378) | `const digest = trimmedInstructionDigest(file.content)` |
| `cwd` | `const` | [`packages/context/agent-instructions/src/index.ts:124`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/context/agent-instructions/src/index.ts#L124) | `const cwd = agent.session.header.cwd ?? process.cwd()` |
| `digest` | `const` | [`packages/context/agent-instructions/src/state.ts:170`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/context/agent-instructions/src/state.ts#L170) | `const digest = instructionContentSha1(file.content)` |
| `cwd` | `const` | [`packages/context/agent-instructions/src/state.ts:264`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/context/agent-instructions/src/state.ts#L264) | `const cwd = session.header.cwd ?? process.cwd()` |
| `FsVersion` | `type` | [`packages/fs/fs/src/types.ts:35`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/fs/src/types.ts#L35) | `export type FsVersion = Branded<'FsVersion'>` |
| `maxBytes` | `const` | [`packages/jobs/tool-jobs/src/index.ts:151`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/jobs/tool-jobs/src/index.ts#L151) | `const maxBytes = snapshot.outputLimitBytes` |
| `streamText` | `function` | [`packages/shell/tool-bash/src/render.ts:12`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/shell/tool-bash/src/render.ts#L12) | `function streamText(output: CollectedOutput): string {` |
| `set` | `const` | [`packages/test-support/client-runtime/src/sessions.ts:52`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/test-support/client-runtime/src/sessions.ts#L52) | `const set = listeners.get(key) ?? new Set()` |
| `replace` | `const` | [`vendor/loader/src/config/entry.ts:194`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/loader/src/config/entry.ts#L194) | `const replace = diff.some(key => key === 'name' \|\| key === 'inject' \|\| key === 'group')` |

### Tests and executable evidence

- [`packages/typert/generator/tests/fixtures/type-model/packages/write/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/typert/generator/tests/fixtures/type-model/packages/write/src/index.ts) — Core file in the package named by the note: `packages/typert/generator/tests/fixtures/type-model/packages/write`.
- [`packages/typert/generator/tests/fixtures/type-model/packages/write`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/typert/generator/tests/fixtures/type-model/packages/write) — The note names this package or capability.
- [`packages/typert/generator/tests/fixtures/type-model/packages/write/package.json`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/typert/generator/tests/fixtures/type-model/packages/write/package.json) — Core file in the package named by the note: `packages/typert/generator/tests/fixtures/type-model/packages/write`.
- [`packages/fs/fs/tests/service.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/fs/tests/service.spec.ts) — A test under the owning area exercises or imports `FsVersion`.
- [`packages/fs/fs/tests/invariant.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/fs/tests/invariant.spec.ts) — A test under the owning area exercises or imports `FsVersion`.
- [`packages/util/home-paths/tests/home-paths.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/util/home-paths/tests/home-paths.spec.ts) — A test under the owning area exercises or imports `$DSH_HOME`. A test under the owning area exercises or imports `dsh-home-paths`.
- [`packages/context/agent-instructions/tests/agent-instructions.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/context/agent-instructions/tests/agent-instructions.e2e.ts) — A test under the owning area exercises or imports `agent-instructions`.
- [`packages/context/agent-instructions/tests/agent-instructions.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/context/agent-instructions/tests/agent-instructions.spec.ts) — A test under the owning area exercises or imports `instructionFileCandidates`. A test under the owning area exercises or imports `localInstructionFileCandidates`.

## How to read the implementation

1. Start with [`packages/context/agent-instructions/README.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/context/agent-instructions/README.md) because it has the strongest evidence link to the note.
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

- Tags: `class/feature`, `concern/boundary`, `concern/cancellation-timeout`, `concern/compatibility`, `concern/concurrency`, `concern/discovery-routing`, `concern/evidence`, `concern/lifecycle`, `concern/ownership`, `concern/performance`, `concern/recovery`, `concern/schema-types`, `concern/simplification`, `concern/trust`, `domain/agent-loop`, `domain/build-release`, `domain/configuration`, `domain/context`, `domain/extensions`, `domain/filesystem`, `domain/llm`, `domain/protocols`, `domain/security`, `domain/session-state`, `domain/shell-terminal`, `domain/storage`, `domain/testing`, `lifecycle/implemented`, `mechanism/event-sourcing`, `mechanism/projection`
- Aliases: `cwd`, `digest`, `FsVersion`, `maxBytes`, `streamText`, `set`, `replace`, `AGENTS.md`, `CLAUDE.md`, `packages/context/agent-instructions`, `@deepseek-ai/dsh-agent-instructions`, `{ maxBytes } | false`, `agent/pre-step`, `tools/result`
- Regex: `(?i)(digest|FsVersion|maxBytes|streamText|replace|AGENTS\.md|CLAUDE\.md|packages/context/agent\-instructions)`

```bash
rg -n --pcre2 "(?i)(digest|FsVersion|maxBytes|streamText|replace|AGENTS\\.md|CLAUDE\\.md|packages/context/agent\\-instructions)" source-deepseek-harness
rg -l --fixed-strings "class/feature" 03-agent-context
```

## Connected notes

- **`source-link`** — [0169. Follow symlinked instruction files](0169-follow-symlinked-instruction-files.md): The source note links to this decision directly.
- **`source-link`** — [0171. Default local instruction overlay](0171-default-local-instruction-overlay.md): The source note links to this decision directly.
- **`shares-code-with`** — [0221. Read card --- the read tool's structured line window reaches the client](0221-read-card-the-read-tool-s-structured-line-window-reaches-the-client.md): Shares source implementation: `packages/context/agent-instructions`, `packages/context/agent-instructions/src/files.ts`.
- **`shares-code-with`** — [0193. tmux-location context](0193-tmux-location-context.md): Shares source implementation: `packages/context/agent-instructions/src/index.ts`, `packages/context/agent-instructions/src/invariant.ts`.
- **`shares-code-with`** — [0252. Web transcript marks context source, recall, and steering](0252-web-transcript-marks-context-source-recall-and-steering.md): Shares source implementation: `packages/context/agent-instructions/src/index.ts`, `packages/context/agent-instructions/src/invariant.ts`.
- **`shares-code-with`** — [0150. Expose agent session identity and JSONL location to tools and hooks](0150-expose-agent-session-identity-and-jsonl-location-to-tools-and-hooks.md): Shares source implementation: `packages/util/home-paths/src/index.ts`, `packages/util/home-paths/src/invariant.ts`.
- **`shares-code-with`** — [0277. User-explicit skill invocation at the pre-step gesture boundary](0277-user-explicit-skill-invocation-at-the-pre-step-gesture-boundary.md): Shares source implementation: `packages/context/agent-instructions`, `packages/context/agent-instructions/src/index.ts`.
- **`shares-code-with`** — [0512. Storage root placement and derived-medium recovery](0512-storage-root-placement-and-derived-medium-recovery.md): Shares source implementation: `packages/util/home-paths/src/index.ts`, `packages/util/home-paths/src/invariant.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0136-workspace-context-instruction-files.md`.
