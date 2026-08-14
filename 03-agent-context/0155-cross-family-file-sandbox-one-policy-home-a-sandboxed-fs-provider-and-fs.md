---
id: "dsh-note-0155"
title: "Cross-family file sandbox --- one policy home, a sandboxed fs provider, and fs escalation parity"
status: "implemented"
class: "feature"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/feature/2026-07-14-cross-family-fs-sandbox.md"
implementation_evidence: "high"
target_anchor: "sandbox capability boundary"
tags:
  - "class/feature"
  - "concern/boundary"
  - "concern/concurrency"
  - "concern/discovery-routing"
  - "concern/evidence"
  - "concern/human-control"
  - "concern/ownership"
  - "concern/recovery"
  - "concern/schema-types"
  - "concern/trust"
  - "domain/build-release"
  - "domain/configuration"
  - "domain/extensions"
  - "domain/filesystem"
  - "domain/llm"
  - "domain/multi-agent"
  - "domain/observability"
  - "domain/protocols"
  - "domain/security"
  - "domain/session-state"
  - "domain/shell-terminal"
  - "domain/storage"
  - "domain/testing"
  - "lifecycle/implemented"
  - "mechanism/policy"
  - "mechanism/registry"
aliases:
  - "web"
  - "shell"
  - "signal"
  - "Config"
  - "mode"
  - "SandboxedFileSystem"
  - "sandboxPolicy"
  - "defaultMode"
  - "cwd"
  - "permission"
  - "SessionEventMap"
  - "approval"
  - "workspaceRoot"
  - "SANDBOX_MODES"
search_regex: "(?i)(shell|signal|Config|mode|SandboxedFileSystem|sandboxPolicy|defaultMode|permission)"
---

# 0155. Cross-family file sandbox --- one policy home, a sandboxed fs provider, and fs escalation parity — implementation context

## Open this when

SandboxMode claims file effects, but originally only ctx.shell enforced it. The fs tools (write/edit) mutate the host filesystem in-process through ctx.fs, where an OS argv wrapper is mechanically meaningless --- the sandbox Agent Note § In-process tools records this and left cross-family enforcement as a deferred phase with an open question: whether in-process enforcement stays per-seam or becomes a uniform harness capability. This Agent Note is that phase, and answers it: one shared policy home, per-seam enforcement at each family's correct altitude. The gap was not read-only-shaped.

## Source decision

Three coordinated pieces, all composed from the leaf cordis.yml, none touching agent-loop. packages/sandbox/sandbox-policy/ (@deepseek-ai/dsh-sandbox-policy) registers ctx.sandboxPolicy, the single owner of the deployment's sandbox policy: Config: mode (the closed SandboxMode union, default read-only) and workspaceRoot (default the process cwd, resolved absolute). Misconfiguration fails loud at load. The per-session override event sandbox/mode, with its pure fold (effectiveSandboxMode(events)), its write path (setSandboxMode(session, mode)), and SANDBOX_MODES.

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/feature/2026-07-14-cross-family-fs-sandbox.md](../02-notes/implemented/feature/2026-07-14-cross-family-fs-sandbox.md)
- Pinned source: [.agents/notes/implemented/feature/2026-07-14-cross-family-fs-sandbox.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/feature/2026-07-14-cross-family-fs-sandbox.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`examples/acp-agent/cordis.yml`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/examples/acp-agent/cordis.yml) | composition and configuration | The source note names this file directly. | `named-file` |
| [`packages/fs/tool-fs/src/edit.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/tool-fs/src/edit.ts) | runtime implementation | Core file in the package named by the note: `packages/fs/tool-fs`. Defines `sandboxPolicy`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/fs/tool-fs/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/tool-fs/src/index.ts) | package entry point | Core file in the package named by the note: `packages/fs/tool-fs`. Defines `Config`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/fs/tool-fs/src/write.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/tool-fs/src/write.ts) | runtime implementation | Core file in the package named by the note: `packages/fs/tool-fs`. Defines `sandboxPolicy`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/fs/fs-local/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/fs-local/src/index.ts) | package entry point | Core file in the package named by the note: `packages/fs/fs-local`. Defines `Config`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/shell/shell/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/shell/shell/src/index.ts) | package entry point | Core file in the package named by the note: `packages/shell/shell`. | `named-package-member` |
| [`packages/shell/shell/src/types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/shell/shell/src/types.ts) | public types and contract | Core file in the package named by the note: `packages/shell/shell`. | `named-package-member` |
| [`packages/fs/tool-fs/src/sandbox.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/tool-fs/src/sandbox.ts) | runtime implementation | Core file in the package named by the note: `packages/fs/tool-fs`. Defines `mode`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/shell/shell/src/render.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/shell/shell/src/render.ts) | runtime implementation | Core file in the package named by the note: `packages/shell/shell`. Defines `signal`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/fs/fs-sandbox/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/fs-sandbox/src/index.ts) | package entry point | Entry point or contract under the directory named by the note: `packages/fs/fs-sandbox`. Core file in the package named by the note: `packages/fs/fs-sandbox`. | `named-directory-member, named-package-member, symbol-definition` |
| [`packages/fs/tool-fs/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/tool-fs/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/fs/tool-fs`. | `named-package-member` |
| [`packages/core/agent-loop/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent-loop/src/index.ts) | package entry point | Core file in the package named by the note: `packages/core/agent-loop`. Defines `Config`, a construct named by the note. | `named-package-member, symbol-definition` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `web` | `const` | [`apps/cli/src/args.ts:156`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/cli/src/args.ts#L156) | `const web = program.command('web').description('boot the web profile (alias of --profile web); the web app\'s own flags follow')` |
| `shell` | `const` | [`packages/client/web/src/boot.tsx:121`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/web/src/boot.tsx#L121) | `const shell = this.ctx.get('appShell')` |
| `signal` | `const` | [`packages/core/agent-loop/src/agent.ts:228`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent-loop/src/agent.ts#L228) | `const signal = this.phase.abort.signal` |
| `Config` | `interface` | [`packages/core/agent-loop/src/index.ts:255`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent-loop/src/index.ts#L255) | `export interface Config {` |
| `mode` | `const` | [`packages/core/agent-loop/src/tool-calls.ts:88`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent-loop/src/tool-calls.ts#L88) | `const mode = ctx.tools.executionMode(first.exec).kind` |
| `Config` | `interface` | [`packages/fs/fs-local/src/index.ts:41`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/fs-local/src/index.ts#L41) | `export interface Config {` |
| `Config` | `type` | [`packages/fs/fs-sandbox/src/index.ts:49`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/fs-sandbox/src/index.ts#L49) | `export type Config = LocalConfig` |
| `SandboxedFileSystem` | `class` | [`packages/fs/fs-sandbox/src/index.ts:59`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/fs-sandbox/src/index.ts#L59) | `export class SandboxedFileSystem extends LocalFileSystem {` |
| `sandboxPolicy` | `const` | [`packages/fs/tool-fs/src/edit.ts:116`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/tool-fs/src/edit.ts#L116) | `const sandboxPolicy = await sandbox.resolvePolicy('edit', args, exec)` |
| `Config` | `interface` | [`packages/fs/tool-fs/src/index.ts:25`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/tool-fs/src/index.ts#L25) | `export interface Config {` |
| `Config` | `const` | [`packages/fs/tool-fs/src/index.ts:36`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/tool-fs/src/index.ts#L36) | `export const Config: z<Config> = z.object({` |
| `defaultMode` | `const` | [`packages/fs/tool-fs/src/sandbox.ts:44`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/tool-fs/src/sandbox.ts#L44) | `const defaultMode = ctx.fs.sandboxMode` |
| `mode` | `const` | [`packages/fs/tool-fs/src/sandbox.ts:128`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/tool-fs/src/sandbox.ts#L128) | `const mode = (policy as SandboxExecutionPolicy).mode` |
| `cwd` | `const` | [`packages/fs/tool-fs/src/session-cwd.ts:24`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/tool-fs/src/session-cwd.ts#L24) | `const cwd = exec.agent?.session.header.cwd` |
| `cwd` | `const` | [`packages/fs/tool-fs/src/session-cwd.ts:41`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/tool-fs/src/session-cwd.ts#L41) | `const cwd = policyWorkspaceRoot ?? sessionCwd(exec, requestedPath)` |
| `sandboxPolicy` | `const` | [`packages/fs/tool-fs/src/write.ts:107`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/tool-fs/src/write.ts#L107) | `const sandboxPolicy = await sandbox.resolvePolicy('write', args, exec)` |

### Tests and executable evidence

- [`packages/typert/generator/tests/fixtures/type-model/packages/write/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/typert/generator/tests/fixtures/type-model/packages/write/src/index.ts) — Core file in the package named by the note: `packages/typert/generator/tests/fixtures/type-model/packages/write`.
- [`packages/typert/generator/tests/fixtures/type-model/packages/write`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/typert/generator/tests/fixtures/type-model/packages/write) — The note names this package or capability.
- [`packages/typert/generator/tests/fixtures/type-model/packages/write/package.json`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/typert/generator/tests/fixtures/type-model/packages/write/package.json) — Core file in the package named by the note: `packages/typert/generator/tests/fixtures/type-model/packages/write`.
- [`apps/web/tests/plan-review.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/plan-review.e2e.ts) — A test under the owning area exercises or imports `approval`.
- [`apps/web/tests/seeded-history.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/seeded-history.e2e.ts) — A test under the owning area exercises or imports `read-only`. A test under the owning area exercises or imports `permission`.
- [`packages/fs/tool-fs/tests/harness.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/tool-fs/tests/harness.ts) — A test under the owning area exercises or imports `dsh-fs-observation-policy`.
- [`apps/web/tests/schedule-after.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/schedule-after.e2e.ts) — A test under the owning area exercises or imports `signal`.
- [`apps/web/tests/settings-chrome.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/settings-chrome.e2e.ts) — A test under the owning area exercises or imports `workspace-write`. A test under the owning area exercises or imports `read-only`.
- Source verification intent: Unit: dsh-sandbox pins the escalation ladder, the marker builders, the argument-pairing validation, and approveEscalation's ordered fail-closed sequence (non-widening, no-approval, no-agent, each outcome), plus writableRoots/canonicalPath. dsh-sandbox-policy pins deployment fallback, session mode/root resolution, explicit-mode precedence, the fold/setter, load-time mode rejection, and HMR safety.

## How to read the implementation

1. Start with [`examples/acp-agent/cordis.yml`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/examples/acp-agent/cordis.yml) because it has the strongest evidence link to the note.
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

- Tags: `class/feature`, `concern/boundary`, `concern/concurrency`, `concern/discovery-routing`, `concern/evidence`, `concern/human-control`, `concern/ownership`, `concern/recovery`, `concern/schema-types`, `concern/trust`, `domain/build-release`, `domain/configuration`, `domain/extensions`, `domain/filesystem`, `domain/llm`, `domain/multi-agent`, `domain/observability`, `domain/protocols`, `domain/security`, `domain/session-state`, `domain/shell-terminal`, `domain/storage`, `domain/testing`, `lifecycle/implemented`, `mechanism/policy`, `mechanism/registry`
- Aliases: `web`, `shell`, `signal`, `Config`, `mode`, `SandboxedFileSystem`, `sandboxPolicy`, `defaultMode`, `cwd`, `permission`, `SessionEventMap`, `approval`, `workspaceRoot`, `SANDBOX_MODES`
- Regex: `(?i)(shell|signal|Config|mode|SandboxedFileSystem|sandboxPolicy|defaultMode|permission)`

```bash
rg -n --pcre2 "(?i)(shell|signal|Config|mode|SandboxedFileSystem|sandboxPolicy|defaultMode|permission)" source-deepseek-harness
rg -l --fixed-strings "class/feature" 03-agent-context
```

## Connected notes

- **`source-link`** — [0146. The subprocess sandbox --- confinement seam, native runners, escalation, and per-session modes](0146-the-subprocess-sandbox-confinement-seam-native-runners-escalation-and-pe.md): The source note links to this decision directly.
- **`shares-code-with`** — [0019. Make `dsh-fs-observation-policy` an event-gate plugin, not a method interface](0019-make-dsh-fs-observation-policy-an-event-gate-plugin-not-a-method-interfa.md): Shares source implementation: `packages/fs/fs-local/src/index.ts`, `packages/fs/tool-fs/src/edit.ts`.
- **`shares-code-with`** — [0528. Prune dead public API and result fields](0528-prune-dead-public-api-and-result-fields.md): Shares source implementation: `packages/fs/fs-local/src/index.ts`, `packages/fs/tool-fs/src/edit.ts`.
- **`shares-code-with`** — [0030. Tool-call timeout policy as a plugin](0030-tool-call-timeout-policy-as-a-plugin.md): Shares source implementation: `packages/shell/shell/src/index.ts`, `packages/shell/shell/src/render.ts`.
- **`shares-code-with`** — [0158. persistent PTY sessions](0158-persistent-pty-sessions.md): Shares source implementation: `packages/shell/shell/src/index.ts`, `packages/shell/shell/src/types.ts`.
- **`shares-code-with`** — [0240. PowerShell executor and pwsh tool](0240-powershell-executor-and-pwsh-tool.md): Shares source implementation: `packages/shell/shell/src/index.ts`, `packages/shell/shell/src/render.ts`.
- **`shares-code-with`** — [0564. Bash-backed grep and glob discovery tools](0564-bash-backed-grep-and-glob-discovery-tools.md): Shares source implementation: `packages/fs/fs-local/src/index.ts`, `packages/fs/tool-fs/src/index.ts`.
- **`shares-code-with`** — [0149. The self-referential cordis toolset](0149-the-self-referential-cordis-toolset.md): Shares source implementation: `packages/shell/shell/src/index.ts`, `packages/shell/shell/src/types.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0155-cross-family-file-sandbox-one-policy-home-a-sandboxed-fs-provider-and-fs.md`.
