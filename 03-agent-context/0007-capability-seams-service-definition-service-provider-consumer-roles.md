---
id: "dsh-note-0007"
title: "Capability seams --- Service Definition / Service Provider / Consumer roles"
status: "implemented"
class: "architecture"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/architecture/2026-06-13-capability-seams.md"
implementation_evidence: "medium"
target_anchor: "sandbox capability boundary"
tags:
  - "class/architecture"
  - "concern/boundary"
  - "concern/compatibility"
  - "concern/discovery-routing"
  - "concern/evidence"
  - "concern/ownership"
  - "concern/schema-types"
  - "concern/trust"
  - "domain/context"
  - "domain/extensions"
  - "domain/llm"
  - "domain/security"
  - "domain/session-state"
  - "domain/shell-terminal"
  - "domain/testing"
  - "domain/tools"
  - "lifecycle/implemented"
  - "mechanism/policy"
  - "mechanism/registry"
aliases:
  - "json"
  - "shell"
  - "consumer"
  - "provider"
  - "inject"
  - "ShellExecutor"
  - "ShellRunResult"
  - "ShellProcess"
  - "Service"
  - "ctx.shell"
  - "ctx.<key>"
  - "dsh-shell"
  - "dsh-bash-local"
  - "dsh-tool-bash"
search_regex: "(?i)(json|shell|consumer|provider|inject|ShellExecutor|ShellRunResult|ShellProcess)"
---

# 0007. Capability seams --- Service Definition / Service Provider / Consumer roles — implementation context

## Open this when

The harness has swappable capabilities --- bash execution today, sandboxed/remote executors and alternative model providers tomorrow. A capability has three concerns that change at different rates and for different reasons: the contract (what the capability is), the implementation (how it runs), and the consumer API (what the model and other plugins program against). Bundling them in one package couples those rates of change --- swapping a local executor for a sandboxed one would churn the tool schemas the model sees, even though the model-facing contract never changed. This is distinct from "who provides vs.

## Source decision

A swappable capability has three roles: Service Definition --- the Cordis Service and vocabulary types owning ctx. and depending only on the vocabulary the contract needs (e.g. dsh-shell: ShellExecutor, ShellRunResult, ShellProcess). A definition may be an abstract class or a concrete registry service; it is never a TypeScript interface. Service Provider --- a plugin that supplies or registers an implementation (e.g. dsh-bash-local: subprocesses, process-group kills, spill-file truncation). Sandboxed and remote providers are sibling packages implementing or registering against the same Service Definition.

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/architecture/2026-06-13-capability-seams.md](../02-notes/implemented/architecture/2026-06-13-capability-seams.md)
- Pinned source: [.agents/notes/implemented/architecture/2026-06-13-capability-seams.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/architecture/2026-06-13-capability-seams.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`AGENTS.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/AGENTS.md) | package contract and examples | The source note names this file directly. Contains the exact code literal `dsh-shell` named by the note. | `exact-code-occurrence, named-file` |
| [`docs/glossary.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/glossary.md) | package contract and examples | The source note names this file directly. Contains the exact code literal `dsh-shell` named by the note. | `exact-code-occurrence, named-file` |
| [`docs/architecture.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/architecture.md) | package contract and examples | The source note names this file directly. Contains the exact code literal `tools/pre-execute` named by the note. | `exact-code-occurrence, named-file` |
| [`packages/llm/llm/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/src/index.ts) | package entry point | Core file in the package named by the note: `packages/llm/llm`. Defines `provider`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/llm/llm/src/types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/src/types.ts) | public types and contract | Core file in the package named by the note: `packages/llm/llm`. | `named-package-member` |
| [`packages/shell/shell/src/types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/shell/shell/src/types.ts) | public types and contract | Entry point or contract under the directory named by the note: `packages/shell`. Core file in the package named by the note: `packages/shell/shell`. | `named-directory-member, named-package-member, symbol-definition` |
| [`packages/shell/shell/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/shell/shell/src/index.ts) | package entry point | Entry point or contract under the directory named by the note: `packages/shell`. Core file in the package named by the note: `packages/shell/shell`. | `named-directory-member, named-package-member, symbol-definition` |
| [`packages/llm/llm/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/llm/llm`. Defines `inject`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/shell/tool-bash/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/shell/tool-bash/src/index.ts) | package entry point | Entry point or contract under the directory named by the note: `packages/shell`. Core file in the package named by the note: `packages/shell/tool-bash`. | `named-directory-member, named-package-member, symbol-definition` |
| [`packages/shell/shell/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/shell/shell/src/invariant.ts) | runtime contract checks | Entry point or contract under the directory named by the note: `packages/shell`. Core file in the package named by the note: `packages/shell/shell`. | `named-directory-member, named-package-member, symbol-definition` |
| [`packages/shell/bash-local/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/shell/bash-local/src/index.ts) | package entry point | Core file in the package named by the note: `packages/shell/bash-local`. | `named-package-member` |
| [`packages/shell/bash-sandbox/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/shell/bash-sandbox/src/index.ts) | package entry point | Core file in the package named by the note: `packages/shell/bash-sandbox`. | `named-package-member` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `json` | `const` | [`packages/client/modules/src/index.ts:169`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/modules/src/index.ts#L169) | `const json = JSON.stringify(graph).replaceAll('<', '\\u003c')` |
| `shell` | `const` | [`packages/client/web/src/boot.tsx:121`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/web/src/boot.tsx#L121) | `const shell = this.ctx.get('appShell')` |
| `consumer` | `const` | [`packages/llm/llm-pi-ai/src/adapter.ts:294`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm-pi-ai/src/adapter.ts#L294) | `const consumer = new AbortController()` |
| `provider` | `const` | [`packages/llm/llm/src/index.ts:632`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/src/index.ts#L632) | `const provider = registration.provider.id` |
| `inject` | `const` | [`packages/llm/llm/src/invariant.ts:12`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/src/invariant.ts#L12) | `export const inject = ['invariants']` |
| `inject` | `const` | [`packages/shell/bash-local/src/invariant.ts:15`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/shell/bash-local/src/invariant.ts#L15) | `export const inject = ['invariants']` |
| `ShellExecutor` | `class` | [`packages/shell/shell/src/index.ts:65`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/shell/shell/src/index.ts#L65) | `export abstract class ShellExecutor extends Service {` |
| `inject` | `const` | [`packages/shell/shell/src/invariant.ts:11`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/shell/shell/src/invariant.ts#L11) | `export const inject = ['invariants']` |
| `ShellRunResult` | `interface` | [`packages/shell/shell/src/types.ts:113`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/shell/shell/src/types.ts#L113) | `export interface ShellRunResult {` |
| `ShellProcess` | `interface` | [`packages/shell/shell/src/types.ts:161`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/shell/shell/src/types.ts#L161) | `export interface ShellProcess {` |
| `inject` | `const` | [`packages/shell/tool-bash/src/index.ts:31`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/shell/tool-bash/src/index.ts#L31) | `export const inject = ['tools', 'shell', 'systemPrompt', 'shellEnv']` |
| `inject` | `const` | [`packages/shell/tool-bash/src/invariant.ts:15`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/shell/tool-bash/src/invariant.ts#L15) | `export const inject = ['invariants']` |
| `Service` | `interface` | [`vendor/cordis/src/reflect.ts:99`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/cordis/src/reflect.ts#L99) | `export interface Service {` |

### Tests and executable evidence

- [`packages/shell/shell/tests/service.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/shell/shell/tests/service.spec.ts) — A test under the owning area exercises or imports `ShellExecutor`. A test under the owning area exercises or imports `ShellRunResult`.
- [`packages/shell/tool-bash/tests/tools.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/shell/tool-bash/tests/tools.spec.ts) — A test under the owning area exercises or imports `ShellExecutor`. A test under the owning area exercises or imports `ShellRunResult`.
- [`packages/shell/bash-sandbox/tests/bwrap.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/shell/bash-sandbox/tests/bwrap.e2e.ts) — A test under the owning area exercises or imports `dsh-bash-sandbox`.
- [`packages/shell/bash-local/tests/settings.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/shell/bash-local/tests/settings.spec.ts) — A test under the owning area exercises or imports `dsh-shell`. A test under the owning area exercises or imports `dsh-bash-local`.
- [`packages/shell/bash-local/tests/executor.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/shell/bash-local/tests/executor.spec.ts) — A test under the owning area exercises or imports `dsh-shell`. A test under the owning area exercises or imports `ShellProcess`.
- [`packages/shell/bash-sandbox/tests/sandbox.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/shell/bash-sandbox/tests/sandbox.spec.ts) — A test under the owning area exercises or imports `ShellRunResult`. A test under the owning area exercises or imports `dsh-bash-sandbox`.
- [`packages/shell/bash-sandbox/tests/landlock.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/shell/bash-sandbox/tests/landlock.e2e.ts) — A test under the owning area exercises or imports `dsh-bash-sandbox`.
- [`packages/shell/bash-sandbox/tests/seatbelt.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/shell/bash-sandbox/tests/seatbelt.e2e.ts) — A test under the owning area exercises or imports `dsh-bash-sandbox`.

## How to read the implementation

1. Start with [`AGENTS.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/AGENTS.md) because it has the strongest evidence link to the note.
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

- Tags: `class/architecture`, `concern/boundary`, `concern/compatibility`, `concern/discovery-routing`, `concern/evidence`, `concern/ownership`, `concern/schema-types`, `concern/trust`, `domain/context`, `domain/extensions`, `domain/llm`, `domain/security`, `domain/session-state`, `domain/shell-terminal`, `domain/testing`, `domain/tools`, `lifecycle/implemented`, `mechanism/policy`, `mechanism/registry`
- Aliases: `json`, `shell`, `consumer`, `provider`, `inject`, `ShellExecutor`, `ShellRunResult`, `ShellProcess`, `Service`, `ctx.shell`, `ctx.<key>`, `dsh-shell`, `dsh-bash-local`, `dsh-tool-bash`
- Regex: `(?i)(json|shell|consumer|provider|inject|ShellExecutor|ShellRunResult|ShellProcess)`

```bash
rg -n --pcre2 "(?i)(json|shell|consumer|provider|inject|ShellExecutor|ShellRunResult|ShellProcess)" source-deepseek-harness
rg -l --fixed-strings "class/architecture" 03-agent-context
```

## Connected notes

- **`shares-code-with`** — [0133. Compaction as a capability seam (abstract contract + basic backend)](0133-compaction-as-a-capability-seam-abstract-contract-basic-backend.md): Shares source implementation: `packages/llm/llm/src/index.ts`, `packages/llm/llm/src/invariant.ts`.
- **`shares-code-with`** — [0014. Branded IDs everywhere they belong](0014-branded-ids-everywhere-they-belong.md): Shares source implementation: `packages/llm/llm/src/index.ts`, `packages/llm/llm/src/types.ts`.
- **`shares-code-with`** — [0240. PowerShell executor and pwsh tool](0240-powershell-executor-and-pwsh-tool.md): Shares source implementation: `packages/shell/shell/src/index.ts`, `packages/shell/shell/src/invariant.ts`.
- **`shares-code-with`** — [0241. Windows defaults to pwsh](0241-windows-defaults-to-pwsh.md): Shares source implementation: `packages/shell/shell/src/index.ts`, `packages/shell/shell/src/invariant.ts`.
- **`shares-code-with`** — [0011. Agent lifecycle and ownership contracts](0011-agent-lifecycle-and-ownership-contracts.md): Shares source implementation: `packages/shell/shell/src/index.ts`, `packages/shell/shell/src/invariant.ts`.
- **`shares-code-with`** — [0260. pwsh UI presentation matches bash](0260-pwsh-ui-presentation-matches-bash.md): Shares source implementation: `packages/shell/shell/src/index.ts`, `packages/shell/shell/src/invariant.ts`.
- **`shares-code-with`** — [0066. The subprocess service is its own seam under the bash executors (`dsh-subprocess` / `dsh-subprocess-local`)](0066-the-subprocess-service-is-its-own-seam-under-the-bash-executors-dsh-subp.md): Shares source implementation: `packages/shell/shell/src/index.ts`, `packages/shell/shell/src/invariant.ts`.
- **`shares-code-with`** — [0131. Code Mode --- the model writes TypeScript against the tool registry](0131-code-mode-the-model-writes-typescript-against-the-tool-registry.md): Shares source implementation: `docs/architecture.md`, `packages/llm/llm/src/index.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0007-capability-seams-service-definition-service-provider-consumer-roles.md`.
