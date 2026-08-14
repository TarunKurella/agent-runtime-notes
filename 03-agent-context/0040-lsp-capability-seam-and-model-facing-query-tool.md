---
id: "dsh-note-0040"
title: "LSP capability seam and model-facing query tool"
status: "implemented"
class: "architecture"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/architecture/2026-07-15-lsp-capability-seam.md"
implementation_evidence: "medium"
target_anchor: "sandbox capability boundary"
tags:
  - "class/architecture"
  - "concern/boundary"
  - "concern/cancellation-timeout"
  - "concern/compatibility"
  - "concern/concurrency"
  - "concern/discovery-routing"
  - "concern/evidence"
  - "concern/human-control"
  - "concern/lifecycle"
  - "concern/ownership"
  - "concern/performance"
  - "concern/recovery"
  - "concern/schema-types"
  - "concern/trust"
  - "domain/build-release"
  - "domain/configuration"
  - "domain/extensions"
  - "domain/filesystem"
  - "domain/llm"
  - "domain/observability"
  - "domain/protocols"
  - "domain/security"
  - "domain/session-state"
  - "domain/shell-terminal"
  - "domain/storage"
  - "domain/testing"
  - "domain/tools"
  - "lifecycle/implemented"
  - "mechanism/event-sourcing"
  - "mechanism/policy"
  - "mechanism/projection"
  - "mechanism/registry"
  - "mechanism/streaming"
aliases:
  - "shutdown"
  - "systemPrompt"
  - "title"
  - "signal"
  - "timeoutMs"
  - "FileLocation"
  - "query"
  - "cwd"
  - "TOOL_TIMEOUT"
  - "hover"
  - "LspPosition"
  - "workspaceRoot"
  - "line"
  - "character"
search_regex: "(?i)(shutdown|systemPrompt|title|signal|timeoutMs|FileLocation|query|TOOL_TIMEOUT)"
---

# 0040. LSP capability seam and model-facing query tool — implementation context

## Open this when

The harness has text search and file reads, but neither identifies a program symbol. A textual match cannot reliably distinguish two same-named functions, follow an import alias, connect an interface to its implementations, or report an inferred type. Before changing code, an agent therefore lacks the semantic navigation that a human gets from an editor's language server. LSP support has three owners: the model needs a stable query schema, the harness needs provider selection and normalized results, and the local implementation needs process, JSON-RPC, workspace, synchronization, and filesystem behavior.

## Source decision

Add LSP as a three-package capability seam with one read-only model tool and one generic local provider implementation: @deepseek-ai/dsh-lsp at packages/lsp/lsp owns ctx.lsp, provider registration and selection, normalized requests/results, execution control, and structured LSP errors. @deepseek-ai/dsh-lsp-stdio at packages/lsp/lsp-stdio adapts configured stdio language servers to the seam. One plugin instance accepts a named server table and registers one isolated provider for each command and extension-to-language-id mapping.

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/architecture/2026-07-15-lsp-capability-seam.md](../02-notes/implemented/architecture/2026-07-15-lsp-capability-seam.md)
- Pinned source: [.agents/notes/implemented/architecture/2026-07-15-lsp-capability-seam.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/architecture/2026-07-15-lsp-capability-seam.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`packages/README.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/README.md) | package contract and examples | The source note names this file directly. | `named-file` |
| [`packages/lsp/lsp/src/types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/lsp/lsp/src/types.ts) | public types and contract | Entry point or contract under the directory named by the note: `packages/lsp`. Entry point or contract under the directory named by the note: `packages/lsp/lsp`. | `named-directory-member, named-package-member, symbol-definition` |
| [`packages/lsp/lsp/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/lsp/lsp/src/index.ts) | package entry point | Entry point or contract under the directory named by the note: `packages/lsp/lsp`. Core file in the package named by the note: `packages/lsp/lsp`. | `named-directory-member, named-package-member` |
| [`packages/core/tools/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/index.ts) | package entry point | Core file in the package named by the note: `packages/core/tools`. Defines `signal`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/core/tools/src/types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/types.ts) | public types and contract | Core file in the package named by the note: `packages/core/tools`. | `named-package-member` |
| [`packages/util/brand/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/util/brand/src/index.ts) | package entry point | Core file in the package named by the note: `packages/util/brand`. | `named-package-member` |
| [`packages/lsp/lsp/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/lsp/lsp/src/invariant.ts) | runtime contract checks | Entry point or contract under the directory named by the note: `packages/lsp/lsp`. Core file in the package named by the note: `packages/lsp/lsp`. | `named-directory-member, named-package-member` |
| [`packages/lsp/tool-lsp/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/lsp/tool-lsp/src/index.ts) | package entry point | Entry point or contract under the directory named by the note: `packages/lsp`. Entry point or contract under the directory named by the note: `packages/lsp/tool-lsp`. | `named-directory-member, named-package-member, symbol-definition` |
| [`packages/lsp/tool-lsp/src/render.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/lsp/tool-lsp/src/render.ts) | runtime implementation | Entry point or contract under the directory named by the note: `packages/lsp`. Entry point or contract under the directory named by the note: `packages/lsp/tool-lsp`. | `named-directory-member, named-package-member, symbol-definition` |
| [`packages/lsp/lsp-stdio/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/lsp/lsp-stdio/src/index.ts) | package entry point | Entry point or contract under the directory named by the note: `packages/lsp/lsp-stdio`. Core file in the package named by the note: `packages/lsp/lsp-stdio`. | `exact-code-occurrence, named-directory-member, named-package-member` |
| [`packages/core/tools/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/core/tools`. | `named-package-member` |
| [`packages/core/tools/src/code-mode.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/code-mode.ts) | runtime implementation | Core file in the package named by the note: `packages/core/tools`. Defines `signal`, a construct named by the note. | `named-package-member, symbol-definition` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `shutdown` | `const` | [`apps/cli/src/profile-boot.ts:210`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/cli/src/profile-boot.ts#L210) | `const shutdown = createProcessShutdown(async () => { await app.current?.fiber.dispose() })` |
| `systemPrompt` | `const` | [`packages/boot/app-boot/src/index.ts:822`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/boot/app-boot/src/index.ts#L822) | `const systemPrompt = ctx.get('systemPrompt')` |
| `title` | `const` | [`packages/client/web/src/app.tsx:32`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/web/src/app.tsx#L32) | `const title = useSessions((state) => {` |
| `signal` | `const` | [`packages/core/tools/src/code-mode.ts:401`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/code-mode.ts#L401) | `const signal = new Promise<void>((resolve) => { wake = resolve })` |
| `timeoutMs` | `const` | [`packages/core/tools/src/index.ts:1046`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/index.ts#L1046) | `const timeoutMs = definition.timeoutMs` |
| `signal` | `const` | [`packages/core/tools/src/index.ts:1372`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/index.ts#L1372) | `const signal = exec.signal` |
| `signal` | `const` | [`packages/core/tools/src/index.ts:1538`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/index.ts#L1538) | `const signal = fused.signal` |
| `FileLocation` | `interface` | [`packages/core/tools/src/presentation.ts:23`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/presentation.ts#L23) | `export interface FileLocation {` |
| `query` | `const` | [`packages/examples/acp-demo/src/index.ts:134`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/examples/acp-demo/src/index.ts#L134) | `const query = ctx.plugin(SqliteSessionQueryEngine, { path: join(persistenceRoot, 'session-query.db') })` |
| `cwd` | `const` | [`packages/fs/tool-fs/src/session-cwd.ts:24`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/tool-fs/src/session-cwd.ts#L24) | `const cwd = exec.agent?.session.header.cwd` |
| `TOOL_TIMEOUT` | `const` | [`packages/guard/timeout-policy/src/index.ts:25`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/guard/timeout-policy/src/index.ts#L25) | `export const TOOL_TIMEOUT = 'TOOL_TIMEOUT'` |
| `timeoutMs` | `const` | [`packages/guard/timeout-policy/src/index.ts:57`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/guard/timeout-policy/src/index.ts#L57) | `const timeoutMs = ctx.tools.get(exec.name, exec.agent)?.timeoutMs` |
| `hover` | `const` | [`packages/lsp/lsp-stdio/src/translate.ts:187`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/lsp/lsp-stdio/src/translate.ts#L187) | `const hover = payload as unknown as WireHover` |
| `LspPosition` | `interface` | [`packages/lsp/lsp/src/types.ts:20`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/lsp/lsp/src/types.ts#L20) | `export interface LspPosition {` |
| `workspaceRoot` | `const` | [`packages/lsp/tool-lsp/src/index.ts:182`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/lsp/tool-lsp/src/index.ts#L182) | `const workspaceRoot = sessionCwd(exec)` |
| `line` | `const` | [`packages/lsp/tool-lsp/src/render.ts:51`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/lsp/tool-lsp/src/render.ts#L51) | `const line = oneBased(args.line, 'line')` |

### Tests and executable evidence

- [`packages/lsp/lsp/tests/lsp.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/lsp/lsp/tests/lsp.spec.ts) — A test under the owning area exercises or imports `goToDefinition`. A test under the owning area exercises or imports `hover`.
- [`apps/cli/tests/process-shutdown.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/cli/tests/process-shutdown.spec.ts) — A test under the owning area exercises or imports `shutdown`.
- [`apps/cli/tests/headless-shutdown.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/cli/tests/headless-shutdown.e2e.ts) — A test under the owning area exercises or imports `shutdown`.
- [`packages/lsp/lsp-stdio/tests/host.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/lsp/lsp-stdio/tests/host.spec.ts) — A test under the owning area exercises or imports `dsh-lsp-stdio`. A test under the owning area exercises or imports `filePath`.
- [`apps/cli/tests/fixtures/never-dispose.mjs`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/cli/tests/fixtures/never-dispose.mjs) — A test under the owning area exercises or imports `shutdown`.
- [`packages/lsp/tool-lsp/tests/render.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/lsp/tool-lsp/tests/render.spec.ts) — A test under the owning area exercises or imports `findReferences`. A test under the owning area exercises or imports `hover`.
- [`packages/core/tools/tests/py-types.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/tests/py-types.spec.ts) — A test under the owning area exercises or imports `character`.
- [`packages/util/timeout/tests/timeout.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/util/timeout/tests/timeout.spec.ts) — A test under the owning area exercises or imports `timeoutOf`.
- Source verification intent: Package tests pin the three-package dependency direction, runtime injections, and ctx.lsp-only communication. Tool tests pin the four operations, coordinate validation, configured bounds and omission markers, prompt, and UI presentation. Registry tests pin atomic reservation/release, order-independent selection, and structured unavailable, disposed, conflict, and unsupported-operation errors. Fake-stdio tests pin exact initialization capabilities, four protocol mappings, Location/LocationLink and hover normalization, and findReferences mapping to references.includeDeclaration.

## How to read the implementation

1. Start with [`packages/README.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/README.md) because it has the strongest evidence link to the note.
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

- Tags: `class/architecture`, `concern/boundary`, `concern/cancellation-timeout`, `concern/compatibility`, `concern/concurrency`, `concern/discovery-routing`, `concern/evidence`, `concern/human-control`, `concern/lifecycle`, `concern/ownership`, `concern/performance`, `concern/recovery`, `concern/schema-types`, `concern/trust`, `domain/build-release`, `domain/configuration`, `domain/extensions`, `domain/filesystem`, `domain/llm`, `domain/observability`, `domain/protocols`, `domain/security`, `domain/session-state`, `domain/shell-terminal`, `domain/storage`, `domain/testing`, `domain/tools`, `lifecycle/implemented`, `mechanism/event-sourcing`, `mechanism/policy`, `mechanism/projection`, `mechanism/registry`, `mechanism/streaming`
- Aliases: `shutdown`, `systemPrompt`, `title`, `signal`, `timeoutMs`, `FileLocation`, `query`, `cwd`, `TOOL_TIMEOUT`, `hover`, `LspPosition`, `workspaceRoot`, `line`, `character`
- Regex: `(?i)(shutdown|systemPrompt|title|signal|timeoutMs|FileLocation|query|TOOL_TIMEOUT)`

```bash
rg -n --pcre2 "(?i)(shutdown|systemPrompt|title|signal|timeoutMs|FileLocation|query|TOOL_TIMEOUT)" source-deepseek-harness
rg -l --fixed-strings "class/architecture" 03-agent-context
```

## Connected notes

- **`source-link`** — [0072. Portable consumers over filesystem and subprocess execution worlds](0072-portable-consumers-over-filesystem-and-subprocess-execution-worlds.md): The source note links to this decision directly.
- **`shares-code-with`** — [0561. Plan mode --- a logged per-agent session mode](0561-plan-mode-a-logged-per-agent-session-mode.md): Shares source implementation: `packages/README.md`, `packages/core/tools/src/index.ts`.
- **`shares-code-with`** — [0049. Canonical tool output contract](0049-canonical-tool-output-contract.md): Shares source implementation: `packages/lsp/lsp/src/index.ts`, `packages/lsp/lsp/src/invariant.ts`.
- **`shares-code-with`** — [0018. Web capability seam - stable tools over multiple providers](0018-web-capability-seam-stable-tools-over-multiple-providers.md): Shares source implementation: `packages/core/tools/src/index.ts`, `packages/core/tools/src/types.ts`.
- **`shares-code-with`** — [0158. persistent PTY sessions](0158-persistent-pty-sessions.md): Shares source implementation: `packages/core/tools/src/index.ts`, `packages/core/tools/src/types.ts`.
- **`shares-code-with`** — [0142. Dynamic workflows --- a script-driven multi-agent orchestration seam](0142-dynamic-workflows-a-script-driven-multi-agent-orchestration-seam.md): Shares source implementation: `packages/core/tools/src/index.ts`, `packages/core/tools/src/types.ts`.
- **`shares-code-with`** — [0564. Bash-backed grep and glob discovery tools](0564-bash-backed-grep-and-glob-discovery-tools.md): Shares source implementation: `packages/core/tools/src/index.ts`, `packages/core/tools/src/types.ts`.
- **`shares-code-with`** — [0144. The approval seam --- one-shot permission decisions over a waterfall of answerers](0144-the-approval-seam-one-shot-permission-decisions-over-a-waterfall-of-answ.md): Shares source implementation: `packages/core/tools/src/index.ts`, `packages/core/tools/src/types.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0040-lsp-capability-seam-and-model-facing-query-tool.md`.
