---
id: "dsh-note-0487"
title: "parseCmdline runs the program's own commander action"
status: "implemented"
class: "simplification"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/simplification/2026-08-11-cmdline-program-action.md"
implementation_evidence: "medium"
target_anchor: "Rust provider-adapter boundary"
tags:
  - "class/simplification"
  - "concern/boundary"
  - "concern/compatibility"
  - "concern/discovery-routing"
  - "concern/evidence"
  - "concern/lifecycle"
  - "concern/ownership"
  - "concern/schema-types"
  - "domain/configuration"
  - "domain/extensions"
  - "domain/llm"
  - "domain/observability"
  - "domain/storage"
  - "lifecycle/implemented"
  - "mechanism/policy"
  - "mechanism/registry"
aliases:
  - "ctx"
  - "CmdlineArgs"
  - "parseCmdline"
  - "exit"
  - "isCommanderError"
  - "resolve"
  - "pending"
  - "dsh-cmdline"
  - "CmdlinePlan<T> = (program, ctx) => T"
  - "CmdlinePlan"
  - "program.error"
  - "(() => ({}) as T)"
  - "exitOverride"
  - "parseCmdline(ctx, program): void"
search_regex: "(?i)(CmdlineArgs|parseCmdline|exit|isCommanderError|resolve|pending|dsh\\-cmdline|CmdlinePlan<T>[- ]=[- ]\\(program,[- ]ctx\\)[- ]=>[- ]T)"
---

# 0487. parseCmdline runs the program's own commander action — implementation context

## Open this when

dsh-cmdline's (app-owned command line) parseCmdline carried a bespoke callback: CmdlinePlan = (program, ctx) => T, invoked after a successful parse inside the helper's catch so a plan's program.error(...) shared the help/parse-error exit path, with a type-unsound (() => ({}) as T) default only tests used and a ctx argument no plan read. The whole seam duplicated a slot commander already defines: a command's action handler runs inside parse, and program.error(...) thrown from it obeys exitOverride exactly like a grammar rejection.

## Source decision

parseCmdline(ctx, program): void only adapts commander control flow to the launcher: it parses the immutable cmdlineArgs snapshot and turns help, version, parse errors, and action rejections into a ctx.appExit request. App code --- validation commander's grammar cannot express and the ctx.provide of the app-owned service --- lives in the program's own synchronous .action(), which commander runs on a successful parse and never runs on help or rejection. The CmdlinePlan export, its ctx parameter, the default plan, and the T | undefined return are deleted; both bundle providers publish from their action.

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/simplification/2026-08-11-cmdline-program-action.md](../02-notes/implemented/simplification/2026-08-11-cmdline-program-action.md)
- Pinned source: [.agents/notes/implemented/simplification/2026-08-11-cmdline-program-action.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/simplification/2026-08-11-cmdline-program-action.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`packages/boot/cmdline/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/boot/cmdline/src/index.ts) | package entry point | Core file in the package named by the note: `packages/boot/cmdline`. Defines `parseCmdline`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/boot/cmdline/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/boot/cmdline/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/boot/cmdline`. | `named-package-member` |
| [`packages/boot/cmdline`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/packages/boot/cmdline) | package or module directory | The note names this package or capability. | `named-package` |
| [`vendor/hmr/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/hmr/src/index.ts) | package entry point | Defines `pending`, a construct named by the note. | `symbol-definition` |
| [`apps/cli/src/profile-boot.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/cli/src/profile-boot.ts) | runtime implementation | Defines `ctx`, a construct named by the note. | `symbol-definition` |
| [`vendor/cordis/src/registry.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/cordis/src/registry.ts) | runtime implementation | Defines `resolve`, a construct named by the note. | `symbol-definition` |
| [`packages/boot/cmdline/README.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/boot/cmdline/README.md) | package contract and examples | Core file in the package named by the note: `packages/boot/cmdline`. | `named-package-member` |
| [`packages/boot/cmdline/package.json`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/boot/cmdline/package.json) | composition and configuration | Core file in the package named by the note: `packages/boot/cmdline`. | `named-package-member` |
| [`pnpm-lock.yaml`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/pnpm-lock.yaml) | composition and configuration | Contains the exact code literal `dsh-cmdline` named by the note. | `exact-code-occurrence` |
| [`apps/cli/README.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/cli/README.md) | package contract and examples | Contains the exact code literal `dsh-cmdline` named by the note. | `exact-code-occurrence` |
| [`apps/web/package.json`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/package.json) | composition and configuration | Contains the exact code literal `dsh-cmdline` named by the note. | `exact-code-occurrence` |
| [`apps/cli/README.zh.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/cli/README.zh.md) | package contract and examples | Contains the exact code literal `dsh-cmdline` named by the note. | `exact-code-occurrence` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `ctx` | `const` | [`apps/cli/src/profile-boot.ts:248`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/cli/src/profile-boot.ts#L248) | `const ctx = await boot(NAME, rootConfig, structuredClone(allPatches(composed)), (hostCtx) => {` |
| `CmdlineArgs` | `interface` | [`packages/boot/cmdline/src/index.ts:27`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/boot/cmdline/src/index.ts#L27) | `export interface CmdlineArgs {` |
| `parseCmdline` | `function` | [`packages/boot/cmdline/src/index.ts:98`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/boot/cmdline/src/index.ts#L98) | `export function parseCmdline(ctx: Context, program: Command): void {` |
| `exit` | `const` | [`packages/boot/cmdline/src/index.ts:102`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/boot/cmdline/src/index.ts#L102) | `const exit = ctx.get('appExit')` |
| `isCommanderError` | `function` | [`packages/boot/cmdline/src/index.ts:167`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/boot/cmdline/src/index.ts#L167) | `function isCommanderError(error: unknown): error is { code: string; exitCode: number } {` |
| `resolve` | `function` | [`vendor/cordis/src/registry.ts:71`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/cordis/src/registry.ts#L71) | `export function resolve(inject: Inject \| null \| undefined, result: Dict = Object.create(null)) {` |
| `pending` | `const` | [`vendor/hmr/src/index.ts:346`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/hmr/src/index.ts#L346) | `const pending: string[] = []` |

### Tests and executable evidence

- [`packages/boot/cmdline/tests/cmdline.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/boot/cmdline/tests/cmdline.spec.ts) — A test under the owning area exercises or imports `parseCmdline`. A test under the owning area exercises or imports `exitOverride`.
- [`apps/web/tests/scaffold.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/scaffold.ts) — Contains the exact code literal `dsh-cmdline` named by the note.

## How to read the implementation

1. Start with [`packages/boot/cmdline/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/boot/cmdline/src/index.ts) because it has the strongest evidence link to the note.
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
- **`rust/trait-seam`** — Use a small trait at the capability boundary; keep policy and model-facing schemas outside the backend implementation.

### Smallest safe implementation slice

1. Express the semantic contract as Rust types and one owning boundary.
2. Add the smallest adapter or service needed by the existing loop; do not add a new model-facing tool unless the four-tool surface cannot express the job.
3. Record durable evidence before deriving summaries or UI state.
4. Add one happy-path test, one interruption test, and one replay/restart test where state survives a turn.
5. Keep the patch reversible until the source note's strongest failure case is proven in the Rust harness.

## Search handles

- Tags: `class/simplification`, `concern/boundary`, `concern/compatibility`, `concern/discovery-routing`, `concern/evidence`, `concern/lifecycle`, `concern/ownership`, `concern/schema-types`, `domain/configuration`, `domain/extensions`, `domain/llm`, `domain/observability`, `domain/storage`, `lifecycle/implemented`, `mechanism/policy`, `mechanism/registry`
- Aliases: `ctx`, `CmdlineArgs`, `parseCmdline`, `exit`, `isCommanderError`, `resolve`, `pending`, `dsh-cmdline`, `CmdlinePlan<T> = (program, ctx) => T`, `CmdlinePlan`, `program.error`, `(() => ({}) as T)`, `exitOverride`, `parseCmdline(ctx, program): void`
- Regex: `(?i)(CmdlineArgs|parseCmdline|exit|isCommanderError|resolve|pending|dsh\-cmdline|CmdlinePlan<T>[- ]=[- ]\(program,[- ]ctx\)[- ]=>[- ]T)`

```bash
rg -n --pcre2 "(?i)(CmdlineArgs|parseCmdline|exit|isCommanderError|resolve|pending|dsh\\-cmdline|CmdlinePlan<T>[- ]=[- ]\\(program,[- ]ctx\\)[- ]=>[- ]T)" source-deepseek-harness
rg -l --fixed-strings "class/simplification" 03-agent-context
```

## Connected notes

- **`source-link`** — [0105. Apps own their command line through `ctx.cmdlineArgs`](0105-apps-own-their-command-line-through-ctx-cmdlineargs.md): The source note links to this decision directly.
- **`shares-code-with`** — [0123. Trim the command-line seams to existing interfaces](0123-trim-the-command-line-seams-to-existing-interfaces.md): Shares source implementation: `packages/boot/cmdline`, `packages/boot/cmdline/src/index.ts`.
- **`shares-code-with`** — [0539. Prune unused skill registry API](0539-prune-unused-skill-registry-api.md): Shares source implementation: `vendor/cordis/src/registry.ts`, `vendor/hmr/src/index.ts`.
- **`shares-code-with`** — [0514. Cordis Host/Client Dynamic Plugin Runtime](0514-cordis-host-client-dynamic-plugin-runtime.md): Shares source implementation: `vendor/cordis/src/registry.ts`.
- **`shares-code-with`** — [0044. GUI layering and the RPC protocol --- host/client layering by capability provider, the four-quadrant message model, and the fetch carrier](0044-gui-layering-and-the-rpc-protocol-host-client-layering-by-capability-pro.md): Shares source implementation: `vendor/hmr/src/index.ts`.
- **`shares-code-with`** — [0388. Export JSDoc gate](0388-export-jsdoc-gate.md): Shares source implementation: `vendor/cordis/src/registry.ts`, `vendor/hmr/src/index.ts`.
- **`shares-code-with`** — [0656. Fold the stdio UI helper into the stdio app](0656-fold-the-stdio-ui-helper-into-the-stdio-app.md): Shares source implementation: `vendor/cordis/src/registry.ts`, `vendor/hmr/src/index.ts`.
- **`shares-code-with`** — [0103. Slot declaration injection and reload lifetimes](0103-slot-declaration-injection-and-reload-lifetimes.md): Shares source implementation: `vendor/cordis/src/registry.ts`, `vendor/hmr/src/index.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0487-parsecmdline-runs-the-program-s-own-commander-action.md`.
