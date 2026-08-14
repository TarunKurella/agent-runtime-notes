---
id: "dsh-note-0229"
title: "Even out the shipped tool rosters"
status: "implemented"
class: "feature"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/feature/2026-07-31-even-out-shipped-tool-rosters.md"
implementation_evidence: "high"
target_anchor: "sandbox capability boundary"
tags:
  - "class/feature"
  - "concern/boundary"
  - "concern/evidence"
  - "concern/human-control"
  - "concern/lifecycle"
  - "concern/ownership"
  - "concern/schema-types"
  - "concern/simplification"
  - "concern/trust"
  - "domain/build-release"
  - "domain/configuration"
  - "domain/extensions"
  - "domain/filesystem"
  - "domain/llm"
  - "domain/protocols"
  - "domain/security"
  - "domain/session-state"
  - "domain/shell-terminal"
  - "domain/storage"
  - "domain/testing"
  - "domain/ui-interaction"
  - "domain/web-retrieval"
  - "lifecycle/implemented"
  - "mechanism/adapter"
  - "mechanism/policy"
  - "mechanism/projection"
  - "mechanism/registry"
  - "mechanism/streaming"
aliases:
  - "code"
  - "json"
  - "dsh"
  - "shell"
  - "command"
  - "tui.cordis.yml"
  - "tool-todo"
  - "web.cordis.yml"
  - "base.cordis.yml"
  - "tool-session-query"
  - "tool-str-replace-editor"
  - "repeat-tool-reminder"
  - "dsh-tool-fs-search"
  - "tmux-context"
search_regex: "(?i)(code|json|shell|command|tui\\.cordis\\.yml|tool\\-todo|web\\.cordis\\.yml|base\\.cordis\\.yml)"
---

# 0229. Even out the shipped tool rosters — implementation context

## Open this when

The two shipped dsh surfaces offered different tools for no recorded reason. Session checkpoints, tool-result pruning, the goal tools, and Ralph were in tui.cordis.yml; tool-todo and, later, web search were in web.cordis.yml. Neither surface offered session search, a string-replacement editor, or a repeat-tool guard, though all three exist as packages and none is surface-specific. The result was a user-visible difference nobody had decided: the same model, asked the same thing, could set a goal on the terminal but not in the browser, and could search the web in the browser but not on the terminal.

## Source decision

The rows that are not surface-specific move into base.cordis.yml, and three more join them: tool-session-query, tool-str-replace-editor, and repeat-tool-reminder. Web search moves there too; its deployment decision owns the security boundary while the shared base owns its surface-neutral mount. Both surfaces assemble the same roster, including fixed glob and grep members because dsh-tool-fs-search spawns the packaged ripgrep binary.

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/feature/2026-07-31-even-out-shipped-tool-rosters.md](../02-notes/implemented/feature/2026-07-31-even-out-shipped-tool-rosters.md)
- Pinned source: [.agents/notes/implemented/feature/2026-07-31-even-out-shipped-tool-rosters.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/feature/2026-07-31-even-out-shipped-tool-rosters.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`packages/bundle/base/cordis.patch.yml`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/bundle/base/cordis.patch.yml) | composition and configuration | The source note names this file directly. | `named-file` |
| [`packages/sandbox/sandbox/src/roots.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/sandbox/sandbox/src/roots.ts) | runtime implementation | The source note names this file directly. | `named-file` |
| [`packages/web/web-fetch-http/README.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/web/web-fetch-http/README.md) | package contract and examples | The source note names this file directly. Core file in the package named by the note: `packages/web/web-fetch-http`. | `named-file, named-package-member` |
| [`packages/extensions/tool-cordis/README.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/extensions/tool-cordis/README.md) | package contract and examples | The source note names this file directly. Core file in the package named by the note: `packages/extensions/tool-cordis`. | `named-file, named-package-member` |
| [`packages/web/web-fetch-http/src/policy.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/web/web-fetch-http/src/policy.ts) | runtime implementation | The source note names this file directly. | `named-file` |
| [`packages/hooks/hooks-claude-code/README.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/hooks/hooks-claude-code/README.md) | package contract and examples | The source note names this file directly. Core file in the package named by the note: `packages/hooks/hooks-claude-code`. | `named-file, named-package-member` |
| [`apps/cli/src/plugin.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/cli/src/plugin.ts) | runtime implementation | Entry point or contract under the directory named by the note: `apps/cli`. Core file in the package named by the note: `apps/cli`. | `named-directory-member, named-package-member, symbol-definition` |
| [`packages/shell/shell/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/shell/shell/src/index.ts) | package entry point | Core file in the package named by the note: `packages/shell/shell`. | `named-package-member` |
| [`packages/shell/shell/src/types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/shell/shell/src/types.ts) | public types and contract | Core file in the package named by the note: `packages/shell/shell`. | `named-package-member` |
| [`packages/web/tool-web/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/web/tool-web/src/index.ts) | package entry point | Core file in the package named by the note: `packages/web/tool-web`. | `named-package-member` |
| [`packages/mcp/mcp-client/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/mcp/mcp-client/src/index.ts) | package entry point | Core file in the package named by the note: `packages/mcp/mcp-client`. | `named-package-member` |
| [`packages/todo/tool-todo/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/todo/tool-todo/src/index.ts) | package entry point | Core file in the package named by the note: `packages/todo/tool-todo`. | `named-package-member` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `code` | `const` | [`apps/cli/src/plugin.ts:135`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/cli/src/plugin.ts#L135) | `const code = (result.error as NodeJS.ErrnoException).code` |
| `json` | `const` | [`packages/client/modules/src/index.ts:169`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/modules/src/index.ts#L169) | `const json = JSON.stringify(graph).replaceAll('<', '\\u003c')` |
| `dsh` | `const` | [`packages/client/modules/src/index.ts:345`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/modules/src/index.ts#L345) | `const dsh = pkg.dsh` |
| `shell` | `const` | [`packages/client/web/src/boot.tsx:121`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/web/src/boot.tsx#L121) | `const shell = this.ctx.get('appShell')` |
| `command` | `const` | [`packages/context/tmux-context/src/index.ts:114`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/context/tmux-context/src/index.ts#L114) | `const command = [` |
| `code` | `const` | [`packages/session-query/tool-session-query/src/service-boundary.ts:126`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/session-query/tool-session-query/src/service-boundary.ts#L126) | `const code: unknown = error.code` |

### Tests and executable evidence

- [`apps/web/tests/shipped-composition.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/shipped-composition.e2e.ts) — The source note names this file directly.
- [`apps/cli/tests/built-bin.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/cli/tests/built-bin.e2e.ts) — A test under the owning area exercises or imports `minimal`. A test under the owning area exercises or imports `PATH`.
- [`apps/cli/tests/windows-shell.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/cli/tests/windows-shell.spec.ts) — A test under the owning area exercises or imports `minimal`.
- [`apps/cli/tests/web-agent-presets.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/cli/tests/web-agent-presets.e2e.ts) — A test under the owning area exercises or imports `tool-todo`. A test under the owning area exercises or imports `glob`.
- [`packages/web/tool-web/tests/spill.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/web/tool-web/tests/spill.spec.ts) — A test under the owning area exercises or imports `dsh-web-fetch-http`. A test under the owning area exercises or imports `dsh-tool-web`.
- [`apps/cli/tests/memory-mcp-configs.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/cli/tests/memory-mcp-configs.spec.ts) — A test under the owning area exercises or imports `dsh-mcp-client`. Contains the exact code literal `dsh-mcp-client` named by the note.
- [`packages/mcp/mcp-client/tests/apply.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/mcp/mcp-client/tests/apply.spec.ts) — A test under the owning area exercises or imports `dsh-mcp-client`.
- [`packages/web/tool-web/tests/tool-web.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/web/tool-web/tests/tool-web.spec.ts) — A test under the owning area exercises or imports `dsh-tool-web`.
- Source verification intent: apps/cli/tests/shipped-composition.e2e.ts booted the shipped tree through the real Loader in a pseudo-terminal and read the tool names out of the request/header the session log persisted, so the assertion was the catalog the model was actually sent. Its --config overlay, composition-keyless-tail.cordis.yml, provided test isolation only: a network-free adapter and workspace-local session artifacts. That tail also inserted composition-settled.ts, which announced settled Loader activation on the terminal stream.

## How to read the implementation

1. Start with [`packages/bundle/base/cordis.patch.yml`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/bundle/base/cordis.patch.yml) because it has the strongest evidence link to the note.
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

- Tags: `class/feature`, `concern/boundary`, `concern/evidence`, `concern/human-control`, `concern/lifecycle`, `concern/ownership`, `concern/schema-types`, `concern/simplification`, `concern/trust`, `domain/build-release`, `domain/configuration`, `domain/extensions`, `domain/filesystem`, `domain/llm`, `domain/protocols`, `domain/security`, `domain/session-state`, `domain/shell-terminal`, `domain/storage`, `domain/testing`, `domain/ui-interaction`, `domain/web-retrieval`, `lifecycle/implemented`, `mechanism/adapter`, `mechanism/policy`, `mechanism/projection`, `mechanism/registry`, `mechanism/streaming`
- Aliases: `code`, `json`, `dsh`, `shell`, `command`, `tui.cordis.yml`, `tool-todo`, `web.cordis.yml`, `base.cordis.yml`, `tool-session-query`, `tool-str-replace-editor`, `repeat-tool-reminder`, `dsh-tool-fs-search`, `tmux-context`
- Regex: `(?i)(code|json|shell|command|tui\.cordis\.yml|tool\-todo|web\.cordis\.yml|base\.cordis\.yml)`

```bash
rg -n --pcre2 "(?i)(code|json|shell|command|tui\\.cordis\\.yml|tool\\-todo|web\\.cordis\\.yml|base\\.cordis\\.yml)" source-deepseek-harness
rg -l --fixed-strings "class/feature" 03-agent-context
```

## Connected notes

- **`source-link`** — [0091. Packaged ripgrep spawn for glob/grep](0091-packaged-ripgrep-spawn-for-glob-grep.md): The source note links to this decision directly.
- **`source-link`** — [0235. Default Web search in shipped compositions](0235-default-web-search-in-shipped-compositions.md): The source note links to this decision directly.
- **`source-link`** — [0238. Workspace-write defaults for shipped surfaces](0238-workspace-write-defaults-for-shipped-surfaces.md): The source note links to this decision directly.
- **`source-link`** — [0243. Session search tools are not a shipped default](0243-session-search-tools-are-not-a-shipped-default.md): The source note links to this decision directly.
- **`shares-code-with`** — [0146. The subprocess sandbox --- confinement seam, native runners, escalation, and per-session modes](0146-the-subprocess-sandbox-confinement-seam-native-runners-escalation-and-pe.md): Shares source implementation: `packages/shell/shell/src/index.ts`, `packages/shell/shell/src/types.ts`.
- **`shares-code-with`** — [0149. The self-referential cordis toolset](0149-the-self-referential-cordis-toolset.md): Shares source implementation: `packages/extensions/tool-cordis/README.md`, `packages/shell/shell/src/index.ts`.
- **`shares-code-with`** — [0158. persistent PTY sessions](0158-persistent-pty-sessions.md): Shares source implementation: `packages/shell/shell/src/index.ts`, `packages/shell/shell/src/types.ts`.
- **`shares-code-with`** — [0155. Cross-family file sandbox --- one policy home, a sandboxed fs provider, and fs escalation parity](0155-cross-family-file-sandbox-one-policy-home-a-sandboxed-fs-provider-and-fs.md): Shares source implementation: `packages/shell/shell/src/index.ts`, `packages/shell/shell/src/types.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0229-even-out-the-shipped-tool-rosters.md`.
