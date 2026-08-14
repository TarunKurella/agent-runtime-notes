---
id: "dsh-note-0072"
title: "Portable consumers over filesystem and subprocess execution worlds"
status: "implemented"
class: "architecture"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/architecture/2026-07-28-portable-execution-world-consumers.md"
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
  - "concern/lifecycle"
  - "concern/ownership"
  - "concern/performance"
  - "concern/recovery"
  - "concern/simplification"
  - "concern/trust"
  - "domain/agent-loop"
  - "domain/build-release"
  - "domain/configuration"
  - "domain/extensions"
  - "domain/filesystem"
  - "domain/llm"
  - "domain/multi-agent"
  - "domain/protocols"
  - "domain/security"
  - "domain/session-state"
  - "domain/shell-terminal"
  - "domain/storage"
  - "domain/testing"
  - "lifecycle/implemented"
  - "mechanism/adapter"
  - "mechanism/policy"
  - "mechanism/registry"
  - "mechanism/streaming"
aliases:
  - "sandbox"
  - "dsh-terminal-bash"
  - "ctx.fs"
  - "ctx.subprocess"
  - "spawnTerminal"
  - "dsh-bash-local"
  - "ctx.subprocess.spawn"
  - "dsh-lsp-stdio"
  - "ctx.subprocess.spawnTerminal"
  - "node-pty"
  - "dsh-subprocess-local"
  - "danger-full-access"
  - "ctx.sandbox"
  - "packages/e2b/"
search_regex: "(?i)(sandbox|dsh\\-terminal\\-bash|ctx\\.fs|ctx\\.subprocess|spawnTerminal|dsh\\-bash\\-local|ctx\\.subprocess\\.spawn|dsh\\-lsp\\-stdio)"
---

# 0072. Portable consumers over filesystem and subprocess execution worlds — implementation context

## Open this when

The filesystem and subprocess seams made file and ordinary process access replaceable, but PTY and LSP still reached host Node APIs directly. A remote execution provider therefore appeared to need separate PTY and LSP packages even though their domain behavior did not change. Those packages would be shallow adapters: each would duplicate an existing consumer merely to replace its file and process operations. A remote coding world is useful only when file operations, commands, terminals, and language servers share one sandbox identity.

## Source decision

ctx.fs and ctx.subprocess together define one execution world. Providers mounted together must describe the same path namespace, executables, processes, and terminal sessions; higher capabilities consume those two interfaces rather than name the provider. The filesystem interface owns the path facts that another capability needs without exposing its opaque target identity: a canonical process path, canonical file: URI, and containment. Existing whole and streaming text operations remain filesystem-owned; protocol consumers enforce their own retention limits while consuming the stream.

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/architecture/2026-07-28-portable-execution-world-consumers.md](../02-notes/implemented/architecture/2026-07-28-portable-execution-world-consumers.md)
- Pinned source: [.agents/notes/implemented/architecture/2026-07-28-portable-execution-world-consumers.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/architecture/2026-07-28-portable-execution-world-consumers.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`packages/e2b/e2b/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/e2b/e2b/src/index.ts) | package entry point | Entry point or contract under the directory named by the note: `packages/e2b`. Core file in the package named by the note: `packages/e2b/e2b`. | `named-directory-member, named-package-member, symbol-definition` |
| [`packages/e2b/fs-e2b/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/e2b/fs-e2b/src/index.ts) | package entry point | Entry point or contract under the directory named by the note: `packages/e2b`. Core file in the package named by the note: `packages/e2b/fs-e2b`. | `named-directory-member, named-package-member, symbol-definition` |
| [`packages/e2b/e2b/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/e2b/e2b/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/e2b/e2b`. | `named-package-member` |
| [`packages/lsp/lsp-stdio/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/lsp/lsp-stdio/src/index.ts) | package entry point | Core file in the package named by the note: `packages/lsp/lsp-stdio`. | `named-package-member` |
| [`packages/e2b/fs-e2b/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/e2b/fs-e2b/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/e2b/fs-e2b`. | `named-package-member` |
| [`packages/sandbox/sandbox/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/sandbox/sandbox/src/index.ts) | package entry point | Core file in the package named by the note: `packages/sandbox/sandbox`. | `named-package-member` |
| [`packages/shell/bash-local/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/shell/bash-local/src/index.ts) | package entry point | Core file in the package named by the note: `packages/shell/bash-local`. | `named-package-member` |
| [`packages/lsp/lsp-stdio/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/lsp/lsp-stdio/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/lsp/lsp-stdio`. | `named-package-member` |
| [`packages/e2b/subprocess-e2b/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/e2b/subprocess-e2b/src/index.ts) | package entry point | Entry point or contract under the directory named by the note: `packages/e2b`. Core file in the package named by the note: `packages/e2b/subprocess-e2b`. | `named-directory-member, named-package-member` |
| [`packages/sandbox/sandbox/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/sandbox/sandbox/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/sandbox/sandbox`. | `named-package-member` |
| [`packages/e2b/subprocess-e2b/src/process.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/e2b/subprocess-e2b/src/process.ts) | runtime implementation | Entry point or contract under the directory named by the note: `packages/e2b`. Core file in the package named by the note: `packages/e2b/subprocess-e2b`. | `named-directory-member, named-package-member` |
| [`packages/shell/bash-local/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/shell/bash-local/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/shell/bash-local`. | `named-package-member` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `sandbox` | `const` | [`packages/e2b/e2b/src/index.ts:132`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/e2b/e2b/src/index.ts#L132) | `const sandbox = await this.ready` |
| `sandbox` | `const` | [`packages/e2b/e2b/src/index.ts:152`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/e2b/e2b/src/index.ts#L152) | `const sandbox = await Sandbox.create({` |
| `sandbox` | `const` | [`packages/e2b/fs-e2b/src/index.ts:181`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/e2b/fs-e2b/src/index.ts#L181) | `const sandbox = await this.ctx.e2b.getSandbox()` |
| `sandbox` | `const` | [`packages/e2b/fs-e2b/src/index.ts:237`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/e2b/fs-e2b/src/index.ts#L237) | `const sandbox = await this.ctx.e2b.getSandbox()` |
| `sandbox` | `const` | [`packages/e2b/fs-e2b/src/index.ts:249`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/e2b/fs-e2b/src/index.ts#L249) | `const sandbox = await this.ctx.e2b.getSandbox()` |

### Tests and executable evidence

- [`packages/e2b/e2b/tests/e2b.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/e2b/e2b/tests/e2b.spec.ts) — A test under the owning area exercises or imports `dsh-e2b`.
- [`packages/e2b/e2b/tests/composition.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/e2b/e2b/tests/composition.e2e.ts) — A test under the owning area exercises or imports `dsh-terminal-bash`. A test under the owning area exercises or imports `danger-full-access`.
- [`packages/lsp/lsp-stdio/tests/host.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/lsp/lsp-stdio/tests/host.spec.ts) — A test under the owning area exercises or imports `dsh-lsp-stdio`.
- [`packages/lsp/lsp-stdio/tests/framing.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/lsp/lsp-stdio/tests/framing.spec.ts) — A test under the owning area exercises or imports `dsh-lsp-stdio`.
- [`packages/e2b/fs-e2b/tests/filesystem.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/e2b/fs-e2b/tests/filesystem.spec.ts) — A test under the owning area exercises or imports `dsh-e2b`. A test under the owning area exercises or imports `dsh-fs-e2b`.
- [`packages/lsp/lsp-stdio/tests/provider.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/lsp/lsp-stdio/tests/provider.spec.ts) — A test under the owning area exercises or imports `dsh-lsp-stdio`. A test under the owning area exercises or imports `dsh-subprocess-local`.
- [`packages/lsp/lsp-stdio/tests/built-lib.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/lsp/lsp-stdio/tests/built-lib.e2e.ts) — A test under the owning area exercises or imports `dsh-lsp-stdio`. A test under the owning area exercises or imports `dsh-subprocess-local`.
- [`packages/lsp/lsp-stdio/tests/instance.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/lsp/lsp-stdio/tests/instance.spec.ts) — A test under the owning area exercises or imports `dsh-lsp-stdio`. A test under the owning area exercises or imports `dsh-subprocess-local`.
- Source verification intent: Focused package suites pin sandbox lifecycle, canonical path framing, filesystem metadata and atomic versions, subprocess publication/rollback, terminal text I/O and session cleanup, output limits, cancellation, disposal, and invariant registration. A credential-gated Loader composition exercises the same three-package provider through source imports and built exports, including FS/Bash visibility, hostile login profiles, byte-split UTF-8 output, process and terminal cleanup, LSP queries, host-workspace isolation, and final sandbox deletion.

## How to read the implementation

1. Start with [`packages/e2b/e2b/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/e2b/e2b/src/index.ts) because it has the strongest evidence link to the note.
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

- Tags: `class/architecture`, `concern/boundary`, `concern/cancellation-timeout`, `concern/compatibility`, `concern/concurrency`, `concern/discovery-routing`, `concern/evidence`, `concern/lifecycle`, `concern/ownership`, `concern/performance`, `concern/recovery`, `concern/simplification`, `concern/trust`, `domain/agent-loop`, `domain/build-release`, `domain/configuration`, `domain/extensions`, `domain/filesystem`, `domain/llm`, `domain/multi-agent`, `domain/protocols`, `domain/security`, `domain/session-state`, `domain/shell-terminal`, `domain/storage`, `domain/testing`, `lifecycle/implemented`, `mechanism/adapter`, `mechanism/policy`, `mechanism/registry`, `mechanism/streaming`
- Aliases: `sandbox`, `dsh-terminal-bash`, `ctx.fs`, `ctx.subprocess`, `spawnTerminal`, `dsh-bash-local`, `ctx.subprocess.spawn`, `dsh-lsp-stdio`, `ctx.subprocess.spawnTerminal`, `node-pty`, `dsh-subprocess-local`, `danger-full-access`, `ctx.sandbox`, `packages/e2b/`
- Regex: `(?i)(sandbox|dsh\-terminal\-bash|ctx\.fs|ctx\.subprocess|spawnTerminal|dsh\-bash\-local|ctx\.subprocess\.spawn|dsh\-lsp\-stdio)`

```bash
rg -n --pcre2 "(?i)(sandbox|dsh\\-terminal\\-bash|ctx\\.fs|ctx\\.subprocess|spawnTerminal|dsh\\-bash\\-local|ctx\\.subprocess\\.spawn|dsh\\-lsp\\-stdio)" source-deepseek-harness
rg -l --fixed-strings "class/architecture" 03-agent-context
```

## Connected notes

- **`shares-code-with`** — [0178. Web UI permission presets and approval answering](0178-web-ui-permission-presets-and-approval-answering.md): Shares source implementation: `packages/sandbox/sandbox/src/index.ts`, `packages/shell/bash-local/src/index.ts`.
- **`shares-code-with`** — [0542. Dependency swaps rejected by the 2026-07 NIH audit](0542-dependency-swaps-rejected-by-the-2026-07-nih-audit.md): Shares source implementation: `packages/lsp/lsp-stdio/src/index.ts`.
- **`shares-code-with`** — [0126. Repository naming contract and pre-release rename ledger](0126-repository-naming-contract-and-pre-release-rename-ledger.md): Shares source implementation: `packages/e2b/e2b/src/index.ts`.
- **`shares-code-with`** — [0040. LSP capability seam and model-facing query tool](0040-lsp-capability-seam-and-model-facing-query-tool.md): Shares source implementation: `packages/lsp/lsp-stdio/src/index.ts`.
- **`shares-code-with`** — [0356. Filesystem absence is an observation and guarded creation never replaces](0356-filesystem-absence-is-an-observation-and-guarded-creation-never-replaces.md): Shares source implementation: `packages/e2b/fs-e2b/src/index.ts`, `packages/e2b/fs-e2b/src/invariant.ts`.
- **`same-design-pressure`** — [0157. Harness-level goal-based execution](0157-harness-level-goal-based-execution.md): Shares design concerns: `concern/boundary`, `concern/cancellation-timeout`, `concern/compatibility`.
- **`same-design-pressure`** — [0146. The subprocess sandbox --- confinement seam, native runners, escalation, and per-session modes](0146-the-subprocess-sandbox-confinement-seam-native-runners-escalation-and-pe.md): Shares design concerns: `concern/boundary`, `concern/compatibility`, `concern/concurrency`.
- **`shares-code-with`** — [0554. dsh-tui chat channel module split](0554-dsh-tui-chat-channel-module-split.md): Shares source implementation: `packages/e2b/e2b/src/index.ts`, `packages/e2b/fs-e2b/src/index.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0072-portable-consumers-over-filesystem-and-subprocess-execution-worlds.md`.
