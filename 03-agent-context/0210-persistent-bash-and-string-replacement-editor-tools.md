---
id: "dsh-note-0210"
title: "Persistent Bash and string-replacement editor tools"
status: "implemented"
class: "feature"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/feature/2026-07-29-persistent-bash-str-replace-editor.md"
implementation_evidence: "high"
target_anchor: "sandbox capability boundary"
tags:
  - "class/feature"
  - "concern/boundary"
  - "concern/cancellation-timeout"
  - "concern/discovery-routing"
  - "concern/evidence"
  - "concern/lifecycle"
  - "concern/ownership"
  - "concern/recovery"
  - "concern/schema-types"
  - "concern/trust"
  - "domain/build-release"
  - "domain/configuration"
  - "domain/extensions"
  - "domain/filesystem"
  - "domain/llm"
  - "domain/security"
  - "domain/session-state"
  - "domain/shell-terminal"
  - "domain/storage"
  - "domain/testing"
  - "domain/web-retrieval"
  - "lifecycle/implemented"
  - "mechanism/policy"
  - "mechanism/registry"
aliases:
  - "terminals"
  - "view"
  - "insert"
  - "str_replace_editor"
  - "@deepseek-ai/dsh-tool-bash-persistent"
  - "ctx.terminals"
  - "[exit code: N]"
  - "[shell exited: code N]"
  - "[shell killed by signal: SIG]"
  - "maxOutputChars"
  - "@deepseek-ai/dsh-tool-str-replace-editor"
  - "ctx.fs"
  - "str_replace"
  - "old_str"
search_regex: "(?i)(terminals|view|insert|str_replace_editor|@deepseek\\-ai/dsh\\-tool\\-bash\\-persistent|ctx\\.terminals|\\[exit[- ]code:[- ]N\\]|\\[shell[- ]exited:[- ]code[- ]N\\])"
---

# 0210. Persistent Bash and string-replacement editor tools — implementation context

## Open this when

Some deployments need a one-call Bash schema whose shell state survives across model turns, while others need a Claude-style str_replace_editor independent of their terminal choice. Bundling the two tools or naming them after one benchmark would prevent reuse and blur configuration ownership.

## Source decision

@deepseek-ai/dsh-tool-bash-persistent consumes ctx.terminals and registers one bash(command) tool. It lazily creates one interactive shell per exact Agent and serializes that owner's calls. Cwd, exported variables, activated environments, functions, and background jobs persist. Random private markers delimit command output. Retained scrollback is paged backward to recover the command's original prefix; a dropped prefix is reported explicitly.

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/feature/2026-07-29-persistent-bash-str-replace-editor.md](../02-notes/implemented/feature/2026-07-29-persistent-bash-str-replace-editor.md)
- Pinned source: [.agents/notes/implemented/feature/2026-07-29-persistent-bash-str-replace-editor.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/feature/2026-07-29-persistent-bash-str-replace-editor.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`apps/cli/config/agent-presets/minimal/agent.cordis.yml`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/cli/config/agent-presets/minimal/agent.cordis.yml) | composition and configuration | The source note names this file directly. | `named-file` |
| [`packages/core/system-prompt/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/system-prompt/src/index.ts) | package entry point | Core file in the package named by the note: `packages/core/system-prompt`. | `named-package-member` |
| [`packages/core/system-prompt/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/system-prompt/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/core/system-prompt`. | `named-package-member` |
| [`packages/examples/agent-spine-demo/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/examples/agent-spine-demo/src/index.ts) | package entry point | Core file in the package named by the note: `packages/examples/agent-spine-demo`. | `named-package-member` |
| [`packages/fs/tool-str-replace-editor/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/tool-str-replace-editor/src/index.ts) | package entry point | Core file in the package named by the note: `packages/fs/tool-str-replace-editor`. | `named-package-member` |
| [`packages/shell/tool-bash-persistent/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/shell/tool-bash-persistent/src/index.ts) | package entry point | Core file in the package named by the note: `packages/shell/tool-bash-persistent`. | `named-package-member` |
| [`packages/examples/agent-spine-demo/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/examples/agent-spine-demo/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/examples/agent-spine-demo`. | `named-package-member` |
| [`packages/fs/tool-str-replace-editor/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/tool-str-replace-editor/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/fs/tool-str-replace-editor`. | `named-package-member` |
| [`packages/shell/tool-bash-persistent/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/shell/tool-bash-persistent/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/shell/tool-bash-persistent`. | `named-package-member` |
| [`packages/core/system-prompt`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/packages/core/system-prompt) | package or module directory | The note names this package or capability. | `named-package` |
| [`packages/examples/agent-spine-demo`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/packages/examples/agent-spine-demo) | package or module directory | The note names this package or capability. | `named-package` |
| [`packages/fs/tool-str-replace-editor`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/packages/fs/tool-str-replace-editor) | package or module directory | The note names this package or capability. | `named-package` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `terminals` | `const` | [`packages/e2b/subprocess-e2b/src/index.ts:81`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/e2b/subprocess-e2b/src/index.ts#L81) | `const terminals = [...this.terminals]` |
| `view` | `const` | [`packages/goal/goal/src/index.ts:535`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/goal/goal/src/index.ts#L535) | `const view = this.view(cache)` |
| `insert` | `const` | [`packages/session-query/session-query-sqlite/src/index.ts:584`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/session-query/session-query-sqlite/src/index.ts#L584) | `const insert = db.prepare(\`` |

### Tests and executable evidence

- [`packages/e2b/subprocess-e2b/tests/terminal.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/e2b/subprocess-e2b/tests/terminal.spec.ts) — A test under the owning area exercises or imports `terminals`.
- [`packages/fs/tool-str-replace-editor/tests/tools.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/tool-str-replace-editor/tests/tools.spec.ts) — A test under the owning area exercises or imports `str_replace_editor`. A test under the owning area exercises or imports `maxOutputChars`.
- [`packages/shell/tool-bash-persistent/tests/tools.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/shell/tool-bash-persistent/tests/tools.spec.ts) — A test under the owning area exercises or imports `maxOutputChars`.
- [`packages/examples/agent-spine-demo/tests/agent-core.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/examples/agent-spine-demo/tests/agent-core.spec.ts) — A test under the owning area exercises or imports `dsh-agent-spine-demo`.

## How to read the implementation

1. Start with [`apps/cli/config/agent-presets/minimal/agent.cordis.yml`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/cli/config/agent-presets/minimal/agent.cordis.yml) because it has the strongest evidence link to the note.
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
- **`rust/tokio-cancellation`** — Thread one cancellation token through async work, close owned resources before reuse, and test cancellation at every await boundary.
- **`rust/trait-seam`** — Use a small trait at the capability boundary; keep policy and model-facing schemas outside the backend implementation.

### Smallest safe implementation slice

1. Express the semantic contract as Rust types and one owning boundary.
2. Add the smallest adapter or service needed by the existing loop; do not add a new model-facing tool unless the four-tool surface cannot express the job.
3. Record durable evidence before deriving summaries or UI state.
4. Add one happy-path test, one interruption test, and one replay/restart test where state survives a turn.
5. Keep the patch reversible until the source note's strongest failure case is proven in the Rust harness.

## Search handles

- Tags: `class/feature`, `concern/boundary`, `concern/cancellation-timeout`, `concern/discovery-routing`, `concern/evidence`, `concern/lifecycle`, `concern/ownership`, `concern/recovery`, `concern/schema-types`, `concern/trust`, `domain/build-release`, `domain/configuration`, `domain/extensions`, `domain/filesystem`, `domain/llm`, `domain/security`, `domain/session-state`, `domain/shell-terminal`, `domain/storage`, `domain/testing`, `domain/web-retrieval`, `lifecycle/implemented`, `mechanism/policy`, `mechanism/registry`
- Aliases: `terminals`, `view`, `insert`, `str_replace_editor`, `@deepseek-ai/dsh-tool-bash-persistent`, `ctx.terminals`, `[exit code: N]`, `[shell exited: code N]`, `[shell killed by signal: SIG]`, `maxOutputChars`, `@deepseek-ai/dsh-tool-str-replace-editor`, `ctx.fs`, `str_replace`, `old_str`
- Regex: `(?i)(terminals|view|insert|str_replace_editor|@deepseek\-ai/dsh\-tool\-bash\-persistent|ctx\.terminals|\[exit[- ]code:[- ]N\]|\[shell[- ]exited:[- ]code[- ]N\])`

```bash
rg -n --pcre2 "(?i)(terminals|view|insert|str_replace_editor|@deepseek\\-ai/dsh\\-tool\\-bash\\-persistent|ctx\\.terminals|\\[exit[- ]code:[- ]N\\]|\\[shell[- ]exited:[- ]code[- ]N\\])" source-deepseek-harness
rg -l --fixed-strings "class/feature" 03-agent-context
```

## Connected notes

- **`source-link`** — [0358. The minimal preset owns the complete RL agent composition](0358-the-minimal-preset-owns-the-complete-rl-agent-composition.md): The source note links to this decision directly.
- **`shares-code-with`** — [0212. Current sandbox policy context](0212-current-sandbox-policy-context.md): Shares source implementation: `packages/core/system-prompt`, `packages/core/system-prompt/src/index.ts`.
- **`shares-code-with`** — [0234. Third-party memory MCP examples](0234-third-party-memory-mcp-examples.md): Shares source implementation: `packages/core/system-prompt`, `packages/core/system-prompt/src/index.ts`.
- **`shares-code-with`** — [0363. Bounded background job admission](0363-bounded-background-job-admission.md): Shares source implementation: `packages/examples/agent-spine-demo/src/index.ts`, `packages/examples/agent-spine-demo/src/invariant.ts`.
- **`shares-code-with`** — [0313. Web agents receive explicit runtime context](0313-web-agents-receive-explicit-runtime-context.md): Shares source implementation: `packages/core/system-prompt`, `packages/core/system-prompt/src/index.ts`.
- **`shares-code-with`** — [0566. Optional time-context plugin](0566-optional-time-context-plugin.md): Shares source implementation: `packages/examples/agent-spine-demo/src/index.ts`, `packages/examples/agent-spine-demo/src/invariant.ts`.
- **`shares-code-with`** — [0193. tmux-location context](0193-tmux-location-context.md): Shares source implementation: `packages/examples/agent-spine-demo/src/index.ts`, `packages/examples/agent-spine-demo/src/invariant.ts`.
- **`shares-code-with`** — [0252. Web transcript marks context source, recall, and steering](0252-web-transcript-marks-context-source-recall-and-steering.md): Shares source implementation: `packages/core/system-prompt/src/index.ts`, `packages/core/system-prompt/src/invariant.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0210-persistent-bash-and-string-replacement-editor-tools.md`.
