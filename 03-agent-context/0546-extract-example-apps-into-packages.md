---
id: "dsh-note-0546"
title: "Extract example apps into packages"
status: "archived"
class: "architecture"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/archived/architecture/2026-06-20-extract-example-app-packages.md"
implementation_evidence: "medium"
target_anchor: "Rust provider-adapter boundary"
tags:
  - "class/architecture"
  - "concern/boundary"
  - "concern/discovery-routing"
  - "concern/evidence"
  - "concern/lifecycle"
  - "concern/ownership"
  - "concern/schema-types"
  - "concern/simplification"
  - "concern/trust"
  - "domain/configuration"
  - "domain/extensions"
  - "domain/llm"
  - "domain/protocols"
  - "domain/session-state"
  - "domain/shell-terminal"
  - "domain/storage"
  - "domain/testing"
  - "domain/ui-interaction"
  - "lifecycle/archived"
  - "mechanism/adapter"
  - "mechanism/event-sourcing"
  - "mechanism/policy"
aliases:
  - "logger"
  - "hmr"
  - "loader"
  - "apply"
  - "core"
  - "main"
  - "bin"
  - "timer"
  - "start.ts"
  - "base.yml"
  - "base-core.yml"
  - "acp-agent/acp-tail.yml"
  - "agent-loop"
  - "session/new"
search_regex: "(?i)(logger|loader|apply|core|main|timer|start\\.ts|base\\.yml)"
---

# 0546. Extract example apps into packages — implementation context

## Open this when

An example folder is supposed to be thin --- the variable wiring of a demo, not the demo's machinery. Before this change it was thick. Each example carried a hand-rolled start.ts boot bootstrap, an infra preamble (timer, and --- for the stdio demos --- logger + hmr), nested includes of three shared YAML fragments (base.yml / base-core.yml / acp-agent/acp-tail.yml), and per-example agent-loop/persistence/system-prompt config. The actual app --- the spine of services every agent needs --- was spread across the leaf and those includes. The leaf configs also owned coupled front doors.

## Source decision

Each example is now mostly an invocation of an app package, splitting the wiring along the existing interface / implementation / consumer seam: the app package owns the composition, the leaf cordis.yml owns only the swappable choices (which LLM adapter, which bash executor, model, prompt, persistence root). @deepseek-ai/dsh-agent-spine-demo (packages/examples/agent-spine-demo) composes the providerless, executor-less, UI-less spine and forwards the loop's agent-list config.

## Decision status

Historical design. Use it to understand an older pressure or failure; locate the replacing decision before implementing anything.

- Raw note: [archived/architecture/2026-06-20-extract-example-app-packages.md](../02-notes/archived/architecture/2026-06-20-extract-example-app-packages.md)
- Pinned source: [.agents/notes/archived/architecture/2026-06-20-extract-example-app-packages.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/archived/architecture/2026-06-20-extract-example-app-packages.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`docs/postmortem/0001-acp-default-export-drops-inject.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/postmortem/0001-acp-default-export-drops-inject.md) | package contract and examples | The source note names this file directly. Contains the exact code literal `session/new` named by the note. | `exact-code-occurrence, named-file` |
| [`vendor/hmr/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/hmr/src/index.ts) | package entry point | Core file in the package named by the note: `vendor/hmr`. | `named-package-member` |
| [`vendor/timer/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/timer/src/index.ts) | package entry point | Core file in the package named by the note: `vendor/timer`. Defines `timer`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`vendor/loader/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/loader/src/index.ts) | package entry point | Core file in the package named by the note: `vendor/loader`. | `named-package-member` |
| [`packages/boot/app-boot/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/boot/app-boot/src/index.ts) | package entry point | Core file in the package named by the note: `packages/boot/app-boot`. Defines `hmr`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/core/agent-loop/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent-loop/src/index.ts) | package entry point | Core file in the package named by the note: `packages/core/agent-loop`. | `named-package-member` |
| [`packages/shell/tool-bash/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/shell/tool-bash/src/index.ts) | package entry point | Core file in the package named by the note: `packages/shell/tool-bash`. Defines `apply`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/shell/bash-local/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/shell/bash-local/src/index.ts) | package entry point | Core file in the package named by the note: `packages/shell/bash-local`. | `named-package-member` |
| [`packages/boot/app-boot/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/boot/app-boot/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/boot/app-boot`. Defines `apply`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/examples/acp-demo/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/examples/acp-demo/src/index.ts) | package entry point | Core file in the package named by the note: `packages/examples/acp-demo`. Defines `apply`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/core/agent-loop/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent-loop/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/core/agent-loop`. Defines `apply`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/shell/tool-bash/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/shell/tool-bash/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/shell/tool-bash`. Defines `apply`, a construct named by the note. | `named-package-member, symbol-definition` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `logger` | `const` | [`packages/acp/acp/src/index.ts:109`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/acp/acp/src/index.ts#L109) | `const logger = ctx.logger` |
| `hmr` | `const` | [`packages/boot/app-boot/src/index.ts:237`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/boot/app-boot/src/index.ts#L237) | `const hmr = ctx.get('hmr')` |
| `loader` | `const` | [`packages/boot/app-boot/src/index.ts:524`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/boot/app-boot/src/index.ts#L524) | `const loader = ctx.get('loader')` |
| `apply` | `const` | [`packages/boot/app-boot/src/invariant.ts:28`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/boot/app-boot/src/invariant.ts#L28) | `export const apply = (ctx: Context): Promise<() => void> =>` |
| `core` | `const` | [`packages/client/ui-conversation/src/client/input/facade.ts:497`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/src/client/input/facade.ts#L497) | `const core = this.core.state` |
| `apply` | `const` | [`packages/core/agent-loop/src/invariant.ts:62`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent-loop/src/invariant.ts#L62) | `export const apply = (ctx: Context): Promise<() => void> =>` |
| `apply` | `function` | [`packages/examples/acp-demo/src/index.ts:113`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/examples/acp-demo/src/index.ts#L113) | `export async function apply(ctx: Context, config: Config): Promise<void> {` |
| `main` | `function` | [`packages/sandbox/sandbox-windows-acl/src/runner.ts:115`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/sandbox/sandbox-windows-acl/src/runner.ts#L115) | `async function main(): Promise<number> {` |
| `apply` | `function` | [`packages/shell/tool-bash/src/index.ts:190`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/shell/tool-bash/src/index.ts#L190) | `export function apply(ctx: Context, config: Config = {}): void {` |
| `apply` | `const` | [`packages/shell/tool-bash/src/invariant.ts:28`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/shell/tool-bash/src/invariant.ts#L28) | `export const apply = (ctx: Context): Promise<() => void> =>` |
| `bin` | `const` | [`scripts/publish-npm-baseline.ts:457`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/publish-npm-baseline.ts#L457) | `const bin = resolve(consumerRoot, 'node_modules/@deepseek-ai/dsh/lib/bin.js')` |
| `timer` | `const` | [`vendor/timer/src/index.ts:36`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/timer/src/index.ts#L36) | `const timer = setTimeout(() => {` |
| `timer` | `const` | [`vendor/timer/src/index.ts:46`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/timer/src/index.ts#L46) | `const timer = setTimeout(resolve, delay)` |
| `timer` | `const` | [`vendor/timer/src/index.ts:64`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/timer/src/index.ts#L64) | `const timer = setInterval(callback, delay)` |
| `timer` | `const` | [`vendor/timer/src/index.ts:71`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/timer/src/index.ts#L71) | `const timer = setInterval(() => {` |

### Tests and executable evidence

- [`packages/core/agent-loop/tests/resume.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent-loop/tests/resume.spec.ts) — A test under the owning area exercises or imports `tool-bash`.
- [`packages/examples/acp-demo/tests/built-bin.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/examples/acp-demo/tests/built-bin.e2e.ts) — A test under the owning area exercises or imports `bash-local`.
- [`packages/examples/agent-spine-demo/tests/agent-core.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/examples/agent-spine-demo/tests/agent-core.spec.ts) — A test under the owning area exercises or imports `dsh-agent-spine-demo`.
- [`packages/shell/tool-bash/tests/tools.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/shell/tool-bash/tests/tools.spec.ts) — Contains the exact code literal `session/new` named by the note.
- Source verification intent: Example directories contain only their config, README, and tests: start.ts, the infrastructure preamble, and the shared YAML includes are gone. demo:tui, demo:headless, and demo:acp invoke the app-package bins. Each new package has a README and per-file 100% coverage; each app package also has a keyless real-Loader-path bin smoke that catches export-shape failures described in postmortem 0001. The ACP replay suite boots through the app-package bin, so protocol wiring and assembled backend behavior cross the real Loader boundary.

## How to read the implementation

1. Start with [`docs/postmortem/0001-acp-default-export-drops-inject.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/postmortem/0001-acp-default-export-drops-inject.md) because it has the strongest evidence link to the note.
2. Read public types and entry points before implementation details. Write down the contract in plain language.
3. Follow the named constructs into their callers and tests. Confirm which component can actually enforce the rule.
4. Reconstruct one failure path: cancellation, crash, retry, stale state, partial write, or incompatible provider input—whichever the note is protecting against.
5. Only then map the contract into the current Rust repository. Target paths must come from that repository, never from guesswork in this wiki.

## Rust harness port

**Likely integration point:** Rust provider-adapter boundary.

Preserve the behavior and ownership rule. Do not mechanically translate Cordis, TypeScript declaration merging, Node streams, or package topology into Rust.

Relevant standing rules:

- **`rust/append-log`** — Keep durable facts append-only and derive views from them. Separate the synchronous fact boundary from asynchronous persistence.
- **`rust/four-tool-surface`** — Preserve the four model tools: read, write, patch, and exec. Reuse internal capabilities without automatically exposing new model-facing tools.
- **`rust/newtype-ids`** — Represent identity domains with Rust newtypes; do not pass bare strings across session, call, turn, step, and agent boundaries.
- **`rust/provider-adapter`** — Keep provider wire quirks inside adapters; the loop and durable event vocabulary remain provider-neutral.
- **`rust/quickjs-boundary`** — QuickJS holds orchestration variables; Rust/Tokio owns async execution and exposes only explicitly granted capabilities.
- **`rust/serde-validation`** — Parse untrusted input into strict serde types and validate semantic constraints before executing side effects.
- **`rust/tagged-enums`** — Use serde-tagged enums for a closed wire vocabulary. Add an explicit extension envelope only where runtime extension is truly required.
- **`rust/tokio-cancellation`** — Thread one cancellation token through async work, close owned resources before reuse, and test cancellation at every await boundary.
- **`rust/trait-seam`** — Use a small trait at the capability boundary; keep policy and model-facing schemas outside the backend implementation.
- **Status gate** — Historical design. Use it to understand an older pressure or failure; locate the replacing decision before implementing anything.

### Smallest safe implementation slice

1. Express the semantic contract as Rust types and one owning boundary.
2. Add the smallest adapter or service needed by the existing loop; do not add a new model-facing tool unless the four-tool surface cannot express the job.
3. Record durable evidence before deriving summaries or UI state.
4. Add one happy-path test, one interruption test, and one replay/restart test where state survives a turn.
5. Keep the patch reversible until the source note's strongest failure case is proven in the Rust harness.

## Search handles

- Tags: `class/architecture`, `concern/boundary`, `concern/discovery-routing`, `concern/evidence`, `concern/lifecycle`, `concern/ownership`, `concern/schema-types`, `concern/simplification`, `concern/trust`, `domain/configuration`, `domain/extensions`, `domain/llm`, `domain/protocols`, `domain/session-state`, `domain/shell-terminal`, `domain/storage`, `domain/testing`, `domain/ui-interaction`, `lifecycle/archived`, `mechanism/adapter`, `mechanism/event-sourcing`, `mechanism/policy`
- Aliases: `logger`, `hmr`, `loader`, `apply`, `core`, `main`, `bin`, `timer`, `start.ts`, `base.yml`, `base-core.yml`, `acp-agent/acp-tail.yml`, `agent-loop`, `session/new`
- Regex: `(?i)(logger|loader|apply|core|main|timer|start\.ts|base\.yml)`

```bash
rg -n --pcre2 "(?i)(logger|loader|apply|core|main|timer|start\\.ts|base\\.yml)" source-deepseek-harness
rg -l --fixed-strings "class/architecture" 03-agent-context
```

## Connected notes

- **`source-link`** — [0547. Reorganize packages into a modular hierarchy](0547-reorganize-packages-into-a-modular-hierarchy.md): The source note links to this decision directly.
- **`source-link`** — [0660. Share the app bins' boot glue instead of maintaining twin copies](0660-share-the-app-bins-boot-glue-instead-of-maintaining-twin-copies.md): The source note links to this decision directly.
- **`shares-code-with`** — [0553. Parse `dsh` argv through one Commander adapter](0553-parse-dsh-argv-through-one-commander-adapter.md): Shares source implementation: `packages/boot/app-boot/src/index.ts`, `packages/boot/app-boot/src/invariant.ts`.
- **`shares-code-with`** — [0573. TUI banner brand gradient](0573-tui-banner-brand-gradient.md): Shares source implementation: `packages/examples/acp-demo/src/index.ts`, `vendor/hmr/src/index.ts`.
- **`shares-code-with`** — [0439. Private npm publication as three independent sequences](0439-private-npm-publication-as-three-independent-sequences.md): Shares source implementation: `vendor/hmr/src/index.ts`, `vendor/loader/src/index.ts`.
- **`shares-code-with`** — [0570. dsh tells the agent where its own source lives](0570-dsh-tells-the-agent-where-its-own-source-lives.md): Shares source implementation: `packages/boot/app-boot/src/index.ts`, `packages/boot/app-boot/src/invariant.ts`.
- **`shares-code-with`** — [0145. Explicit model-facing tool order](0145-explicit-model-facing-tool-order.md): Shares source implementation: `packages/boot/app-boot/src/index.ts`, `packages/boot/app-boot/src/invariant.ts`.
- **`shares-code-with`** — [0602. dsh --dump-config prints the composed config tree](0602-dsh-dump-config-prints-the-composed-config-tree.md): Shares source implementation: `packages/boot/app-boot/src/index.ts`, `packages/boot/app-boot/src/invariant.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0546-extract-example-apps-into-packages.md`.
