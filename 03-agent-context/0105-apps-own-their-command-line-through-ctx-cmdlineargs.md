---
id: "dsh-note-0105"
title: "Apps own their command line through `ctx.cmdlineArgs`"
status: "implemented"
class: "architecture"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/architecture/2026-08-06-app-owned-command-line.md"
implementation_evidence: "medium"
target_anchor: "read/write/patch filesystem capability"
tags:
  - "class/architecture"
  - "concern/boundary"
  - "concern/compatibility"
  - "concern/lifecycle"
  - "concern/ownership"
  - "concern/recovery"
  - "concern/schema-types"
  - "domain/build-release"
  - "domain/configuration"
  - "domain/extensions"
  - "domain/filesystem"
  - "domain/llm"
  - "domain/observability"
  - "domain/protocols"
  - "domain/session-state"
  - "domain/storage"
  - "domain/ui-interaction"
  - "lifecycle/implemented"
aliases:
  - "patches"
  - "web"
  - "plugin"
  - "runProfile"
  - "connection"
  - "key"
  - "inject"
  - "provideCmdline"
  - "parseCmdline"
  - "get"
  - "path"
  - "config"
  - "ctx.cmdlineArgs"
  - "cmdlineArgs"
search_regex: "(?i)(patches|plugin|runProfile|connection|inject|provideCmdline|parseCmdline|path)"
---

# 0105. Apps own their command line through `ctx.cmdlineArgs` — implementation context

## Open this when

After profiles, compositions were installable but their command lines were not. apps/cli still declared the Web flag family (--host, --port, --dev, --workspace-root, --trusted-host) and the one-shot task positional, then derived patches for row ids it hardcoded (webserver, api-gateway, connection, web-runtime). An out-of-tree app such as turtle-ui could contribute rows but had no way to accept a flag: dsh --profile tui --resume had nowhere to be parsed, and dsh --profile web --help printed the launcher's help rather than the web app's.

## Source decision

The launcher parses only what it owns --- --profile, --patch, the config dumps --- and hands everything after its own flags to the booted tree verbatim. The split is positional: the first token the launcher does not recognize starts the app's arguments (commander's passThroughOptions + allowUnknownOption + helpOption(false)). A bare dsh -h, which has no app to hand the flag to, still prints the launcher's own help. The new @deepseek-ai/dsh-cmdline package owns the handoff.

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/architecture/2026-08-06-app-owned-command-line.md](../02-notes/implemented/architecture/2026-08-06-app-owned-command-line.md)
- Pinned source: [.agents/notes/implemented/architecture/2026-08-06-app-owned-command-line.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/architecture/2026-08-06-app-owned-command-line.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`packages/api/gateway/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/api/gateway/src/index.ts) | package entry point | Core file in the package named by the note: `packages/api/gateway`. Defines `key`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/api/gateway/src/types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/api/gateway/src/types.ts) | public types and contract | Core file in the package named by the note: `packages/api/gateway`. | `named-package-member` |
| [`packages/boot/cmdline/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/boot/cmdline/src/index.ts) | package entry point | Core file in the package named by the note: `packages/boot/cmdline`. Defines `provideCmdline`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/bundle/web-app/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/bundle/web-app/src/index.ts) | package entry point | Core file in the package named by the note: `packages/bundle/web-app`. Defines `inject`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/host/webserver/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/webserver/src/index.ts) | package entry point | Core file in the package named by the note: `packages/host/webserver`. | `named-package-member` |
| [`packages/api/gateway/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/api/gateway/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/api/gateway`. Defines `inject`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/bundle/headless/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/bundle/headless/src/index.ts) | package entry point | Core file in the package named by the note: `packages/bundle/headless`. Defines `inject`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/boot/cmdline/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/boot/cmdline/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/boot/cmdline`. Defines `inject`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/bundle/web-app/src/startup.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/bundle/web-app/src/startup.ts) | runtime implementation | Core file in the package named by the note: `packages/bundle/web-app`. Defines `inject`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/bundle/headless/src/startup.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/bundle/headless/src/startup.ts) | runtime implementation | Core file in the package named by the note: `packages/bundle/headless`. | `named-package-member` |
| [`packages/client/connection/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/connection/src/index.ts) | package entry point | Core file in the package named by the note: `packages/client/connection`. Defines `connection`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/api/gateway/src/client/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/api/gateway/src/client/index.ts) | package entry point | Core file in the package named by the note: `packages/api/gateway`. Defines `connection`, a construct named by the note. | `named-package-member, symbol-definition` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `patches` | `const` | [`apps/cli/src/args.ts:84`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/cli/src/args.ts#L84) | `const patches = options.patch ?? []` |
| `web` | `const` | [`apps/cli/src/args.ts:156`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/cli/src/args.ts#L156) | `const web = program.command('web').description('boot the web profile (alias of --profile web); the web app\'s own flags follow')` |
| `plugin` | `const` | [`apps/cli/src/args.ts:171`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/cli/src/args.ts#L171) | `const plugin = program.command('plugin').description('manage a profile\'s plugins by forwarding the remaining arguments to pnpm in the profile directory')` |
| `runProfile` | `function` | [`apps/cli/src/profile-boot.ts:207`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/cli/src/profile-boot.ts#L207) | `export async function runProfile(options: RunProfileOptions): Promise<{ ctx: Context; shutdown: ProcessShutdown }> {` |
| `connection` | `const` | [`packages/api/gateway/src/client/index.ts:399`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/api/gateway/src/client/index.ts#L399) | `const connection = this.ownerCtx.get('connection') as ConnectionHandle \| undefined` |
| `key` | `const` | [`packages/api/gateway/src/index.ts:419`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/api/gateway/src/index.ts#L419) | `const key = parameter.lookup` |
| `inject` | `const` | [`packages/api/gateway/src/invariant.ts:15`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/api/gateway/src/invariant.ts#L15) | `export const inject = ['invariants']` |
| `provideCmdline` | `function` | [`packages/boot/cmdline/src/index.ts:68`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/boot/cmdline/src/index.ts#L68) | `export function provideCmdline(ctx: Context, host: CmdlineHost): void {` |
| `parseCmdline` | `function` | [`packages/boot/cmdline/src/index.ts:98`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/boot/cmdline/src/index.ts#L98) | `export function parseCmdline(ctx: Context, program: Command): void {` |
| `inject` | `const` | [`packages/boot/cmdline/src/invariant.ts:14`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/boot/cmdline/src/invariant.ts#L14) | `export const inject = ['invariants']` |
| `inject` | `const` | [`packages/bundle/headless/src/index.ts:28`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/bundle/headless/src/index.ts#L28) | `export const inject = ['agentDefaultModel', 'agents', 'sessions']` |
| `inject` | `const` | [`packages/bundle/web-app/src/index.ts:35`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/bundle/web-app/src/index.ts#L35) | `export const inject = ['webServer']` |
| `inject` | `const` | [`packages/bundle/web-app/src/startup.ts:17`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/bundle/web-app/src/startup.ts#L17) | `export const inject = ['cmdlineArgs']` |
| `connection` | `const` | [`packages/client/connection/src/index.ts:138`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/connection/src/index.ts#L138) | `const connection = new HostConnectionService(ctx, trustedHosts)` |
| `get` | `const` | [`packages/shell/tool-bash-persistent/src/index.ts:227`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/shell/tool-bash-persistent/src/index.ts#L227) | `const get = (owner: Agent, signal: AbortSignal): Promise<TerminalSessionId> => {` |
| `path` | `const` | [`vendor/hmr/src/index.ts:514`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/hmr/src/index.ts#L514) | `const path = relative(this.baseDir, fileURLToPath(filename))` |

### Tests and executable evidence

- [`apps/web/tests/scaffold.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/scaffold.ts) — A test under the owning area exercises or imports `webserver`. A test under the owning area exercises or imports `api-gateway`.
- [`apps/cli/tests/args.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/cli/tests/args.spec.ts) — A test under the owning area exercises or imports `patches`.
- [`apps/web/tests/assembled-boot.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/assembled-boot.ts) — A test under the owning area exercises or imports `api-gateway`. A test under the owning area exercises or imports `connection`.
- [`apps/web/tests/smoke-real.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/smoke-real.e2e.ts) — A test under the owning area exercises or imports `connection`.
- [`apps/cli/tests/windows-shell.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/cli/tests/windows-shell.spec.ts) — A test under the owning area exercises or imports `patches`.
- [`apps/web/tests/built-boot.snapshot.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/built-boot.snapshot.ts) — A test under the owning area exercises or imports `connection`. A test under the owning area exercises or imports `inject`.
- [`apps/web/tests/models-settings.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/models-settings.e2e.ts) — A test under the owning area exercises or imports `patches`.
- [`apps/web/tests/live-interactions.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/live-interactions.e2e.ts) — A test under the owning area exercises or imports `patches`.

## How to read the implementation

1. Start with [`packages/api/gateway/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/api/gateway/src/index.ts) because it has the strongest evidence link to the note.
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
- **`rust/four-tool-surface`** — Preserve the four model tools: read, write, patch, and exec. Reuse internal capabilities without automatically exposing new model-facing tools.
- **`rust/newtype-ids`** — Represent identity domains with Rust newtypes; do not pass bare strings across session, call, turn, step, and agent boundaries.
- **`rust/process-discipline`** — Treat this as repository or release policy unless it changes a runtime contract. Do not add runtime machinery for a process-only lesson.
- **`rust/provider-adapter`** — Keep provider wire quirks inside adapters; the loop and durable event vocabulary remain provider-neutral.
- **`rust/quickjs-boundary`** — QuickJS holds orchestration variables; Rust/Tokio owns async execution and exposes only explicitly granted capabilities.
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

- Tags: `class/architecture`, `concern/boundary`, `concern/compatibility`, `concern/lifecycle`, `concern/ownership`, `concern/recovery`, `concern/schema-types`, `domain/build-release`, `domain/configuration`, `domain/extensions`, `domain/filesystem`, `domain/llm`, `domain/observability`, `domain/protocols`, `domain/session-state`, `domain/storage`, `domain/ui-interaction`, `lifecycle/implemented`
- Aliases: `patches`, `web`, `plugin`, `runProfile`, `connection`, `key`, `inject`, `provideCmdline`, `parseCmdline`, `get`, `path`, `config`, `ctx.cmdlineArgs`, `cmdlineArgs`
- Regex: `(?i)(patches|plugin|runProfile|connection|inject|provideCmdline|parseCmdline|path)`

```bash
rg -n --pcre2 "(?i)(patches|plugin|runProfile|connection|inject|provideCmdline|parseCmdline|path)" source-deepseek-harness
rg -l --fixed-strings "class/architecture" 03-agent-context
```

## Connected notes

- **`shares-code-with`** — [0060. dsh web config-tree boot and the web transport layering](0060-dsh-web-config-tree-boot-and-the-web-transport-layering.md): Shares source implementation: `packages/api/gateway/src/index.ts`, `packages/api/gateway/src/invariant.ts`.
- **`shares-code-with`** — [0092. Typert Gateway Targeted Method Calls](0092-typert-gateway-targeted-method-calls.md): Shares source implementation: `packages/api/gateway/src/index.ts`, `packages/api/gateway/src/types.ts`.
- **`shares-code-with`** — [0054. Client plugin loading --- plain packages, dsh.client plugins, and the two-phase boot](0054-client-plugin-loading-plain-packages-dsh-client-plugins-and-the-two-phas.md): Shares source implementation: `packages/api/gateway/src/index.ts`, `packages/api/gateway/src/types.ts`.
- **`shares-code-with`** — [0123. Trim the command-line seams to existing interfaces](0123-trim-the-command-line-seams-to-existing-interfaces.md): Shares source implementation: `packages/boot/cmdline/src/index.ts`, `packages/boot/cmdline/src/invariant.ts`.
- **`shares-code-with`** — [0115. headless is a direct core entry point](0115-headless-is-a-direct-core-entry-point.md): Shares source implementation: `packages/bundle/headless/src/index.ts`, `packages/bundle/web-app/src/index.ts`.
- **`shares-code-with`** — [0487. parseCmdline runs the program's own commander action](0487-parsecmdline-runs-the-program-s-own-commander-action.md): Shares source implementation: `packages/boot/cmdline/src/index.ts`, `packages/boot/cmdline/src/invariant.ts`.
- **`shares-code-with`** — [0101. Profile plugin bundles replace the fixed surface overlays](0101-profile-plugin-bundles-replace-the-fixed-surface-overlays.md): Shares source implementation: `packages/bundle/headless/src/index.ts`, `packages/bundle/web-app/src/index.ts`.
- **`shares-code-with`** — [0553. Parse `dsh` argv through one Commander adapter](0553-parse-dsh-argv-through-one-commander-adapter.md): Shares source implementation: `packages/host/webserver/src/index.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0105-apps-own-their-command-line-through-ctx-cmdlineargs.md`.
