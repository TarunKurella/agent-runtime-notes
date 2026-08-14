---
id: "dsh-note-0209"
title: "Adaptive default for the directory-picker interaction"
status: "implemented"
class: "feature"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/feature/2026-07-29-directory-picker-adaptive-default.md"
implementation_evidence: "high"
target_anchor: "append-only event and session store"
tags:
  - "class/feature"
  - "concern/boundary"
  - "concern/evidence"
  - "concern/human-control"
  - "concern/performance"
  - "concern/simplification"
  - "domain/build-release"
  - "domain/configuration"
  - "domain/extensions"
  - "domain/session-state"
  - "domain/storage"
  - "domain/testing"
  - "domain/ui-interaction"
  - "domain/web-retrieval"
  - "lifecycle/implemented"
  - "mechanism/policy"
aliases:
  - "port"
  - "apply"
  - "host"
  - "disabled"
  - "BACKEND_PACKAGES"
  - "native"
  - "pick"
  - "PatchOptions"
  - "cordis.yml"
  - "-browse"
  - "-native"
  - "dsh-host-directory-picker-auto"
  - "httpServer"
  - "SSH_CONNECTION"
search_regex: "(?i)(port|apply|host|disabled|BACKEND_PACKAGES|native|pick|PatchOptions)"
---

# 0209. Adaptive default for the directory-picker interaction — implementation context

## Open this when

The directory-picker seam made the interaction a cordis.yml swap point, but the shipped composition still had to pin one backend: -browse everywhere meant a local operator never got the OS chooser, -native everywhere breaks every remote deployment. The right default depends on facts only the running host knows --- where the server binds, whether the process was launched over SSH, whether a display session exists --- so no static row is correct for all deployments.

## Source decision

A third sibling package, dsh-host-directory-picker-auto: a node-half-only chooser that owns no picking code and no UI. Its apply samples the host facts exactly once at boot --- bind host from the injected httpServer (a new host getter mirrors the existing port), SSH_CONNECTION/SSH_TTY, platform, DISPLAY/WAYLAND_DISPLAY, and a PATH probe for a Linux chooser binary (zenity/kdialog) --- resolves them through one exported pure function, and mounts the chosen dual-face backend with ctx.loader.create({name}) into the Loader's in-memory root tree; the effect's disposer removes the entry and joins the backend fiber's.

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/feature/2026-07-29-directory-picker-adaptive-default.md](../02-notes/implemented/feature/2026-07-29-directory-picker-adaptive-default.md)
- Pinned source: [.agents/notes/implemented/feature/2026-07-29-directory-picker-adaptive-default.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/feature/2026-07-29-directory-picker-adaptive-default.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`packages/client/modules/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/modules/src/index.ts) | package entry point | Core file in the package named by the note: `packages/client/modules`. | `named-package-member` |
| [`packages/host/webserver/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/webserver/src/index.ts) | package entry point | Core file in the package named by the note: `packages/host/webserver`. | `named-package-member` |
| [`packages/client/modules/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/modules/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/client/modules`. Defines `apply`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/host/webserver/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/webserver/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/host/webserver`. Defines `apply`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/client/modules/src/client/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/modules/src/client/index.ts) | package entry point | Core file in the package named by the note: `packages/client/modules`. Defines `apply`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/host/directory-picker/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/directory-picker/src/index.ts) | package entry point | Core file in the package named by the note: `packages/host/directory-picker`. | `named-package-member` |
| [`packages/host/directory-picker/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/directory-picker/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/host/directory-picker`. Defines `apply`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/host/directory-picker-auto/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/directory-picker-auto/src/index.ts) | package entry point | Core file in the package named by the note: `packages/host/directory-picker-auto`. Defines `apply`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/host/directory-picker-auto/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/directory-picker-auto/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/host/directory-picker-auto`. | `named-package-member` |
| [`apps/cli/README.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/cli/README.md) | package contract and examples | Entry point or contract under the directory named by the note: `apps/cli`. | `named-directory-member` |
| [`apps/cli/package.json`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/cli/package.json) | composition and configuration | Entry point or contract under the directory named by the note: `apps/cli`. | `named-directory-member` |
| [`apps/cli`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/apps/cli) | package or module directory | The source note names this implementation area directly. | `named-directory` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `port` | `const` | [`packages/bundle/web-app/src/index.ts:110`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/bundle/web-app/src/index.ts#L110) | `const port = ctx.get('webServer')?.port` |
| `apply` | `function` | [`packages/client/modules/src/client/index.ts:26`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/modules/src/client/index.ts#L26) | `export function apply(ctx: Context): void {` |
| `host` | `const` | [`packages/client/modules/src/invariant.ts:28`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/modules/src/invariant.ts#L28) | `const host = ctx.get('clientModules')` |
| `apply` | `const` | [`packages/client/modules/src/invariant.ts:43`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/modules/src/invariant.ts#L43) | `export const apply = (ctx: Context): Promise<() => void> =>` |
| `disabled` | `const` | [`packages/client/ui-settings-plugins/src/client/BashCard.tsx:23`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-settings-plugins/src/client/BashCard.tsx#L23) | `const disabled = !state.writable` |
| `BACKEND_PACKAGES` | `const` | [`packages/host/directory-picker-auto/src/index.ts:37`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/directory-picker-auto/src/index.ts#L37) | `export const BACKEND_PACKAGES: Record<DirectoryPickerBackendKind, string> = {` |
| `apply` | `function` | [`packages/host/directory-picker-auto/src/index.ts:62`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/directory-picker-auto/src/index.ts#L62) | `export async function apply(ctx: Context): Promise<void> {` |
| `apply` | `const` | [`packages/host/directory-picker/src/invariant.ts:24`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/directory-picker/src/invariant.ts#L24) | `export const apply = (ctx: Context): Promise<() => void> =>` |
| `apply` | `const` | [`packages/host/webserver/src/invariant.ts:57`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/webserver/src/invariant.ts#L57) | `export const apply = (ctx: Context): Promise<() => void> =>` |
| `native` | `const` | [`packages/sandbox/sandbox-windows-acl/src/index.ts:359`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/sandbox/sandbox-windows-acl/src/index.ts#L359) | `const native = spawnSandboxedInherited(api, token, { command: options.command, args, cwd })` |
| `pick` | `function` | [`vendor/cosmokit/src/misc.ts:52`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/cosmokit/src/misc.ts#L52) | `export function pick<T extends object, K extends keyof T>(source: T, keys?: Iterable<K>, forced?: boolean) {` |
| `PatchOptions` | `interface` | [`vendor/include/src/index.ts:145`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/include/src/index.ts#L145) | `export interface PatchOptions {` |

### Tests and executable evidence

- [`apps/web/tests/scaffold.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/scaffold.ts) — The source note names this file directly.
- [`packages/typert/generator/tests/fixtures/type-model/packages/host/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/typert/generator/tests/fixtures/type-model/packages/host/src/index.ts) — Core file in the package named by the note: `packages/typert/generator/tests/fixtures/type-model/packages/host`.
- [`packages/typert/generator/tests/fixtures/type-model/packages/write/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/typert/generator/tests/fixtures/type-model/packages/write/src/index.ts) — Core file in the package named by the note: `packages/typert/generator/tests/fixtures/type-model/packages/write`.
- [`packages/typert/generator/tests/fixtures/type-model/packages/host`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/typert/generator/tests/fixtures/type-model/packages/host) — The note names this package or capability.
- [`packages/typert/generator/tests/fixtures/type-model/packages/write`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/typert/generator/tests/fixtures/type-model/packages/write) — The note names this package or capability.
- [`packages/typert/generator/tests/fixtures/type-model/packages/host/package.json`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/typert/generator/tests/fixtures/type-model/packages/host/package.json) — Core file in the package named by the note: `packages/typert/generator/tests/fixtures/type-model/packages/host`.
- [`packages/typert/generator/tests/fixtures/type-model/packages/write/package.json`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/typert/generator/tests/fixtures/type-model/packages/write/package.json) — Core file in the package named by the note: `packages/typert/generator/tests/fixtures/type-model/packages/write`.
- [`packages/host/directory-picker/tests/seam.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/directory-picker/tests/seam.spec.ts) — A test under the owning area exercises or imports `directoryPicker`.

## How to read the implementation

1. Start with [`packages/client/modules/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/modules/src/index.ts) because it has the strongest evidence link to the note.
2. Read public types and entry points before implementation details. Write down the contract in plain language.
3. Follow the named constructs into their callers and tests. Confirm which component can actually enforce the rule.
4. Reconstruct one failure path: cancellation, crash, retry, stale state, partial write, or incompatible provider input—whichever the note is protecting against.
5. Only then map the contract into the current Rust repository. Target paths must come from that repository, never from guesswork in this wiki.

## Rust harness port

**Likely integration point:** append-only event and session store.

Preserve the behavior and ownership rule. Do not mechanically translate Cordis, TypeScript declaration merging, Node streams, or package topology into Rust.

Relevant standing rules:

- **`rust/append-log`** — Keep durable facts append-only and derive views from them. Separate the synchronous fact boundary from asynchronous persistence.
- **`rust/four-tool-surface`** — Preserve the four model tools: read, write, patch, and exec. Reuse internal capabilities without automatically exposing new model-facing tools.
- **`rust/process-discipline`** — Treat this as repository or release policy unless it changes a runtime contract. Do not add runtime machinery for a process-only lesson.
- **`rust/quickjs-boundary`** — QuickJS holds orchestration variables; Rust/Tokio owns async execution and exposes only explicitly granted capabilities.
- **`rust/trait-seam`** — Use a small trait at the capability boundary; keep policy and model-facing schemas outside the backend implementation.

### Smallest safe implementation slice

1. Express the semantic contract as Rust types and one owning boundary.
2. Add the smallest adapter or service needed by the existing loop; do not add a new model-facing tool unless the four-tool surface cannot express the job.
3. Record durable evidence before deriving summaries or UI state.
4. Add one happy-path test, one interruption test, and one replay/restart test where state survives a turn.
5. Keep the patch reversible until the source note's strongest failure case is proven in the Rust harness.

## Search handles

- Tags: `class/feature`, `concern/boundary`, `concern/evidence`, `concern/human-control`, `concern/performance`, `concern/simplification`, `domain/build-release`, `domain/configuration`, `domain/extensions`, `domain/session-state`, `domain/storage`, `domain/testing`, `domain/ui-interaction`, `domain/web-retrieval`, `lifecycle/implemented`, `mechanism/policy`
- Aliases: `port`, `apply`, `host`, `disabled`, `BACKEND_PACKAGES`, `native`, `pick`, `PatchOptions`, `cordis.yml`, `-browse`, `-native`, `dsh-host-directory-picker-auto`, `httpServer`, `SSH_CONNECTION`
- Regex: `(?i)(port|apply|host|disabled|BACKEND_PACKAGES|native|pick|PatchOptions)`

```bash
rg -n --pcre2 "(?i)(port|apply|host|disabled|BACKEND_PACKAGES|native|pick|PatchOptions)" source-deepseek-harness
rg -l --fixed-strings "class/feature" 03-agent-context
```

## Connected notes

- **`source-link`** — [0070. A capability-discriminated directory-picker seam for the web-GUI host](0070-a-capability-discriminated-directory-picker-seam-for-the-web-gui-host.md): The source note links to this decision directly.
- **`shares-code-with`** — [0198. Web subagent catalog and human continuation](0198-web-subagent-catalog-and-human-continuation.md): Shares source implementation: `packages/host/webserver/src/index.ts`, `packages/host/webserver/src/invariant.ts`.
- **`shares-code-with`** — [0553. Parse `dsh` argv through one Commander adapter](0553-parse-dsh-argv-through-one-commander-adapter.md): Shares source implementation: `packages/host/webserver/src/index.ts`, `packages/host/webserver/src/invariant.ts`.
- **`shares-code-with`** — [0500. Keyless browser e2e lane for the web GUI](0500-keyless-browser-e2e-lane-for-the-web-gui.md): Shares source implementation: `apps/web/tests/scaffold.ts`.
- **`shares-code-with`** — [0099. WebSocket carrier for browser downlinks](0099-websocket-carrier-for-browser-downlinks.md): Shares source implementation: `packages/host/webserver/src/index.ts`, `packages/host/webserver/src/invariant.ts`.
- **`shares-code-with`** — [0060. dsh web config-tree boot and the web transport layering](0060-dsh-web-config-tree-boot-and-the-web-transport-layering.md): Shares source implementation: `packages/client/modules/src/index.ts`, `packages/host/webserver/src/index.ts`.
- **`shares-code-with`** — [0527. Artifact-first NPM baseline publication](0527-artifact-first-npm-baseline-publication.md): Shares source implementation: `packages/client/modules/src/index.ts`.
- **`shares-code-with`** — [0567. Dedicated full-screen TUI front door](0567-dedicated-full-screen-tui-front-door.md): Shares source implementation: `packages/client/modules/src/index.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0209-adaptive-default-for-the-directory-picker-interaction.md`.
