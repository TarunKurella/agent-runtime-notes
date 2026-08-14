---
id: "dsh-note-0607"
title: "experimental subcommands gate behind `--experimental` or `DSH_EXPERIMENTAL=1`"
status: "archived"
class: "feature"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/archived/feature/2026-07-31-experimental-subcommand-gate.md"
implementation_evidence: "medium"
target_anchor: "repository tests and release policy"
tags:
  - "class/feature"
  - "concern/boundary"
  - "concern/evidence"
  - "concern/ownership"
  - "concern/performance"
  - "concern/schema-types"
  - "domain/build-release"
  - "domain/filesystem"
  - "domain/llm"
  - "domain/observability"
  - "domain/testing"
  - "domain/ui-interaction"
  - "lifecycle/archived"
  - "mechanism/adapter"
  - "mechanism/policy"
aliases:
  - "parseDshArgs"
  - "loadEnv"
  - "meta"
  - "env"
  - "--experimental"
  - "DSH_EXPERIMENTAL=1"
  - "dsh experimental-meta"
  - "dsh experimental-upgrade"
  - "args.spec.ts"
  - "bin.ts"
  - "process.env.DSH_EXPERIMENTAL === '1"
  - ".env"
  - "requireExperimental"
  - "built-bin.e2e.ts"
search_regex: "(?i)(parseDshArgs|loadEnv|meta|\\-\\-experimental|DSH_EXPERIMENTAL=1|dsh[- ]experimental\\-meta|dsh[- ]experimental\\-upgrade|args\\.spec\\.ts)"
---

# 0607. experimental subcommands gate behind `--experimental` or `DSH_EXPERIMENTAL=1` — implementation context

## Open this when

The meta and upgrade entry points carried their experimental status in their names: dsh experimental-meta and dsh experimental-upgrade. The prefix made every invocation verbose, and renaming a command at stabilization would break every reference to it --- muscle memory, scripts, and docs alike. The status belongs in an opt-in gate, not in the name.

## Source decision

dsh experimental-meta is dsh meta and dsh experimental-upgrade is dsh upgrade. Each runs only when the invocation passes its --experimental flag or the environment carries DSH_EXPERIMENTAL=1; otherwise the command fails loud on stderr with exit 1, naming both opt-ins. Per the pre-release stance, the old names are gone with no aliases, and args.spec.ts pins their rejection. The gate has two halves with one owner each. The per-invocation half is a Commander --experimental option on each experimental subcommand, checked inside its action after the leaked-parent-option rejection.

## Decision status

Historical design. Use it to understand an older pressure or failure; locate the replacing decision before implementing anything.

- Raw note: [archived/feature/2026-07-31-experimental-subcommand-gate.md](../02-notes/archived/feature/2026-07-31-experimental-subcommand-gate.md)
- Pinned source: [.agents/notes/archived/feature/2026-07-31-experimental-subcommand-gate.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/archived/feature/2026-07-31-experimental-subcommand-gate.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`apps/cli/src/args.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/cli/src/args.ts) | runtime implementation | Defines `parseDshArgs`, a construct named by the note. | `symbol-definition` |
| [`vendor/cordis/src/fiber.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/cordis/src/fiber.ts) | runtime implementation | Defines `meta`, a construct named by the note. | `symbol-definition` |
| [`packages/fs/tool-fs/src/read.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/tool-fs/src/read.ts) | runtime implementation | Defines `meta`, a construct named by the note. | `symbol-definition` |
| [`packages/core/session/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/index.ts) | package entry point | Defines `meta`, a construct named by the note. | `symbol-definition` |
| [`packages/web/tool-web/src/fetch.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/web/tool-web/src/fetch.ts) | runtime implementation | Defines `meta`, a construct named by the note. | `symbol-definition` |
| [`packages/web/tool-web/src/search.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/web/tool-web/src/search.ts) | runtime implementation | Defines `meta`, a construct named by the note. | `symbol-definition` |
| [`packages/boot/app-boot/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/boot/app-boot/src/index.ts) | package entry point | Defines `loadEnv`, a construct named by the note. | `symbol-definition` |
| [`packages/subprocess/subprocess/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/subprocess/subprocess/src/index.ts) | package entry point | Defines `env`, a construct named by the note. | `symbol-definition` |
| [`packages/test-support/loader-smoke/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/test-support/loader-smoke/src/index.ts) | package entry point | Defines `env`, a construct named by the note. | `symbol-definition` |
| [`packages/host/apiproxy/src/native-path-opener.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/src/native-path-opener.ts) | runtime implementation | Defines `env`, a construct named by the note. | `symbol-definition` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `parseDshArgs` | `function` | [`apps/cli/src/args.ts:112`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/cli/src/args.ts#L112) | `export function parseDshArgs(argv: readonly string[], version: string): DshInvocation {` |
| `loadEnv` | `function` | [`packages/boot/app-boot/src/index.ts:78`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/boot/app-boot/src/index.ts#L78) | `export function loadEnv(` |
| `meta` | `const` | [`packages/core/session/src/index.ts:876`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/index.ts#L876) | `const meta = options?.meta` |
| `meta` | `const` | [`packages/fs/tool-fs/src/read.ts:174`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/tool-fs/src/read.ts#L174) | `const meta = readMetaFromMeta(result.meta)` |
| `env` | `const` | [`packages/host/apiproxy/src/native-path-opener.ts:95`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/src/native-path-opener.ts#L95) | `const env = internals.env ?? process.env` |
| `env` | `const` | [`packages/host/apiproxy/src/native-path-opener.ts:127`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/src/native-path-opener.ts#L127) | `const env = internals.env ?? process.env` |
| `env` | `const` | [`packages/host/apiproxy/src/native-path-opener.ts:170`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/src/native-path-opener.ts#L170) | `const env = internals.env ?? process.env` |
| `env` | `const` | [`packages/subprocess/subprocess/src/index.ts:61`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/subprocess/subprocess/src/index.ts#L61) | `const env: Record<string, string> = {}` |
| `env` | `const` | [`packages/test-support/loader-smoke/src/index.ts:110`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/test-support/loader-smoke/src/index.ts#L110) | `const env: NodeJS.ProcessEnv = { ...options.env }` |
| `meta` | `const` | [`packages/web/tool-web/src/fetch.ts:407`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/web/tool-web/src/fetch.ts#L407) | `const meta = fetchMetaFromResult(result.meta)` |
| `meta` | `const` | [`packages/web/tool-web/src/search.ts:61`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/web/tool-web/src/search.ts#L61) | `const meta: string[] = []` |
| `meta` | `const` | [`vendor/cordis/src/fiber.ts:444`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/cordis/src/fiber.ts#L444) | `const meta: EffectMeta = { label, children: [] }` |

### Tests and executable evidence

- [`apps/cli/tests/args.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/cli/tests/args.spec.ts) — A test under the owning area exercises or imports `parseDshArgs`.
- [`apps/cli/tests/built-bin.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/cli/tests/built-bin.e2e.ts) — A test under the owning area exercises or imports `upgrade`.
- [`packages/boot/app-boot/tests/app-boot.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/boot/app-boot/tests/app-boot.spec.ts) — A test under the owning area exercises or imports `loadEnv`.
- [`packages/host/webserver/tests/webserver.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/webserver/tests/webserver.spec.ts) — A test under the owning area exercises or imports `upgrade`.
- [`packages/client/connection/tests/node-half.host.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/connection/tests/node-half.host.spec.ts) — A test under the owning area exercises or imports `upgrade`.
- [`packages/sandbox/sandbox-local/tests/acl-grants.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/sandbox/sandbox-local/tests/acl-grants.spec.ts) — A test under the owning area exercises or imports `upgrade`.
- [`packages/host/apiproxy/tests/api-proxy-agent-preset.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/tests/api-proxy-agent-preset.spec.ts) — A test under the owning area exercises or imports `upgrade`.
- [`packages/session/session-persistence-jsonl/tests/jsonl.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/session/session-persistence-jsonl/tests/jsonl.spec.ts) — A test under the owning area exercises or imports `upgrade`.
- Source verification intent: args.spec.ts pins both admit paths, bare-name rejection, old-name rejection, and leaked-option rejection under the env opt-in. built-bin.e2e.ts proves the assembled entry end to end: the gate diagnostic on stderr with exit 1, and that --experimental, DSH_EXPERIMENTAL=1, but not DSH_EXPERIMENTAL=0, reach the TUI's piped-stdio refusal --- the next gate past this one. Both gated commands were also verified interactively in tmux: dsh meta --experimental and DSH_EXPERIMENTAL=1 dsh meta boot the TUI over the checkout, and DSH_EXPERIMENTAL=1 dsh upgrade seeds the dsh-upgrade skill.

## How to read the implementation

1. Start with [`apps/cli/src/args.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/cli/src/args.ts) because it has the strongest evidence link to the note.
2. Read public types and entry points before implementation details. Write down the contract in plain language.
3. Follow the named constructs into their callers and tests. Confirm which component can actually enforce the rule.
4. Reconstruct one failure path: cancellation, crash, retry, stale state, partial write, or incompatible provider input—whichever the note is protecting against.
5. Only then map the contract into the current Rust repository. Target paths must come from that repository, never from guesswork in this wiki.

## Rust harness port

**Likely integration point:** repository tests and release policy.

Preserve the behavior and ownership rule. Do not mechanically translate Cordis, TypeScript declaration merging, Node streams, or package topology into Rust.

Relevant standing rules:

- **`rust/capability-security`** — Make granted capabilities explicit. On the Windows no-admin path, probe support at runtime and fail closed rather than silently running unsandboxed.
- **`rust/four-tool-surface`** — Preserve the four model tools: read, write, patch, and exec. Reuse internal capabilities without automatically exposing new model-facing tools.
- **`rust/newtype-ids`** — Represent identity domains with Rust newtypes; do not pass bare strings across session, call, turn, step, and agent boundaries.
- **`rust/process-discipline`** — Treat this as repository or release policy unless it changes a runtime contract. Do not add runtime machinery for a process-only lesson.
- **`rust/provider-adapter`** — Keep provider wire quirks inside adapters; the loop and durable event vocabulary remain provider-neutral.
- **`rust/serde-validation`** — Parse untrusted input into strict serde types and validate semantic constraints before executing side effects.
- **`rust/tagged-enums`** — Use serde-tagged enums for a closed wire vocabulary. Add an explicit extension envelope only where runtime extension is truly required.
- **`rust/trait-seam`** — Use a small trait at the capability boundary; keep policy and model-facing schemas outside the backend implementation.
- **Status gate** — Historical design. Use it to understand an older pressure or failure; locate the replacing decision before implementing anything.

### Smallest safe implementation slice

1. Express the semantic contract as Rust types and one owning boundary.
2. Add the smallest adapter or service needed by the existing loop; do not add a new model-facing tool unless the four-tool surface cannot express the job.
3. Record durable evidence before deriving summaries or UI state.
4. Add one happy-path test, one interruption test, and one replay/restart test where state survives a turn.
5. Keep the patch reversible until the source note's strongest failure case is proven in the Rust harness.

## Search handles

- Tags: `class/feature`, `concern/boundary`, `concern/evidence`, `concern/ownership`, `concern/performance`, `concern/schema-types`, `domain/build-release`, `domain/filesystem`, `domain/llm`, `domain/observability`, `domain/testing`, `domain/ui-interaction`, `lifecycle/archived`, `mechanism/adapter`, `mechanism/policy`
- Aliases: `parseDshArgs`, `loadEnv`, `meta`, `env`, `--experimental`, `DSH_EXPERIMENTAL=1`, `dsh experimental-meta`, `dsh experimental-upgrade`, `args.spec.ts`, `bin.ts`, `process.env.DSH_EXPERIMENTAL === '1`, `.env`, `requireExperimental`, `built-bin.e2e.ts`
- Regex: `(?i)(parseDshArgs|loadEnv|meta|\-\-experimental|DSH_EXPERIMENTAL=1|dsh[- ]experimental\-meta|dsh[- ]experimental\-upgrade|args\.spec\.ts)`

```bash
rg -n --pcre2 "(?i)(parseDshArgs|loadEnv|meta|\\-\\-experimental|DSH_EXPERIMENTAL=1|dsh[- ]experimental\\-meta|dsh[- ]experimental\\-upgrade|args\\.spec\\.ts)" source-deepseek-harness
rg -l --fixed-strings "class/feature" 03-agent-context
```

## Connected notes

- **`shares-code-with`** — [0553. Parse `dsh` argv through one Commander adapter](0553-parse-dsh-argv-through-one-commander-adapter.md): Shares source implementation: `apps/cli/src/args.ts`, `packages/boot/app-boot/src/index.ts`.
- **`shares-code-with`** — [0075. Regroup packages/ by measured clustering](0075-regroup-packages-by-measured-clustering.md): Shares source implementation: `packages/boot/app-boot/src/index.ts`, `packages/core/session/src/index.ts`.
- **`shares-code-with`** — [0221. Read card --- the read tool's structured line window reaches the client](0221-read-card-the-read-tool-s-structured-line-window-reaches-the-client.md): Shares source implementation: `packages/fs/tool-fs/src/read.ts`, `vendor/cordis/src/fiber.ts`.
- **`shares-code-with`** — [0166. The dsh CLI and personal config overlays from the Harness home](0166-the-dsh-cli-and-personal-config-overlays-from-the-harness-home.md): Shares source implementation: `apps/cli/src/args.ts`, `packages/boot/app-boot/src/index.ts`.
- **`shares-code-with`** — [0556. Native TypeScript source launch for dsh](0556-native-typescript-source-launch-for-dsh.md): Shares source implementation: `apps/cli/src/args.ts`, `packages/boot/app-boot/src/index.ts`.
- **`shares-code-with`** — [0548. Result-time applied-hunk diffs for file mutations](0548-result-time-applied-hunk-diffs-for-file-mutations.md): Shares source implementation: `packages/fs/tool-fs/src/read.ts`, `vendor/cordis/src/fiber.ts`.
- **`shares-code-with`** — [0528. Prune dead public API and result fields](0528-prune-dead-public-api-and-result-fields.md): Shares source implementation: `packages/core/session/src/index.ts`, `packages/fs/tool-fs/src/read.ts`.
- **`shares-code-with`** — [0597. `dsh meta` boots the TUI over the harness checkout](0597-dsh-meta-boots-the-tui-over-the-harness-checkout.md): Shares source implementation: `apps/cli/src/args.ts`, `vendor/cordis/src/fiber.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0607-experimental-subcommands-gate-behind-experimental-or-dsh-experimental-1.md`.
