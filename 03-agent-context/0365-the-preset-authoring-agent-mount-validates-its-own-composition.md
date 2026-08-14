---
id: "dsh-note-0365"
title: "The preset-authoring agent mount-validates its own composition"
status: "implemented"
class: "bug-fix"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/bug-fix/2026-08-11-preset-authoring-agent-validates-its-own-composition.md"
implementation_evidence: "medium"
target_anchor: "sandbox capability boundary"
tags:
  - "class/bug-fix"
  - "concern/boundary"
  - "concern/discovery-routing"
  - "concern/evidence"
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
  - "domain/multi-agent"
  - "domain/security"
  - "domain/session-state"
  - "domain/shell-terminal"
  - "domain/testing"
  - "domain/tools"
  - "lifecycle/implemented"
  - "mechanism/generation"
  - "mechanism/registry"
aliases:
  - "user"
  - "copy"
  - "standard"
  - "list"
  - "inject"
  - "writableRoot"
  - "broken"
  - "AgentPresets"
  - "files"
  - "row"
  - "resolve"
  - "editing-cordis-compositions"
  - "tool-bash"
  - "bashEnv"
search_regex: "(?i)(user|copy|standard|list|inject|writableRoot|broken|AgentPresets)"
---

# 0365. The preset-authoring agent mount-validates its own composition — implementation context

## Open this when

The cordis preset ships editing-cordis-compositions, the only guidance an agent has when it authors a preset. Four of its statements were false, and the two that carried the most weight pointed at the rule the skill itself calls "the rule that catches people". It named tool-bash as the worked example of a row whose name hides a service --- "reads like a tool but provides bashEnv". tool-bash provides nothing; it declares inject: ['tools', 'bash', 'systemPrompt', 'bashEnv'], and bashEnv comes from the host composition's own shell-env row.

## Source decision

The skill teaches the agent to mount-validate its own composition through ctx.agentPresets, and every remaining example is taken from a shipped composition in the same repository. standingKeyFor(id) is the check. It runs ensureStanding() --- the same real mount a session start performs, minus the agent --- so it rejects a row whose package does not resolve, a row whose config is invalid, a service published into the root realm, and a row that never activated.

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/bug-fix/2026-08-11-preset-authoring-agent-validates-its-own-composition.md](../02-notes/implemented/bug-fix/2026-08-11-preset-authoring-agent-validates-its-own-composition.md)
- Pinned source: [.agents/notes/implemented/bug-fix/2026-08-11-preset-authoring-agent-validates-its-own-composition.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/bug-fix/2026-08-11-preset-authoring-agent-validates-its-own-composition.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`packages/skill/skill-filesystem/README.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/skill/skill-filesystem/README.md) | package contract and examples | The source note names this file directly. Core file in the package named by the note: `packages/skill/skill-filesystem`. | `named-file, named-package-member` |
| [`vendor/cordis/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/cordis/src/index.ts) | package entry point | Core file in the package named by the note: `vendor/cordis`. | `named-package-member` |
| [`vendor/cordis/src/fiber.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/cordis/src/fiber.ts) | runtime implementation | Core file in the package named by the note: `vendor/cordis`. | `named-package-member` |
| [`vendor/cordis/src/registry.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/cordis/src/registry.ts) | runtime implementation | Core file in the package named by the note: `vendor/cordis`. Defines `inject`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/jobs/tool-jobs/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/jobs/tool-jobs/src/index.ts) | package entry point | Core file in the package named by the note: `packages/jobs/tool-jobs`. Defines `inject`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/jobs/jobs-local/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/jobs/jobs-local/src/index.ts) | package entry point | Core file in the package named by the note: `packages/jobs/jobs-local`. | `named-package-member` |
| [`packages/shell/shell-env/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/shell/shell-env/src/index.ts) | package entry point | Core file in the package named by the note: `packages/shell/shell-env`. Defines `inject`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/shell/tool-bash/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/shell/tool-bash/src/index.ts) | package entry point | Core file in the package named by the note: `packages/shell/tool-bash`. Defines `inject`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/jobs/tool-jobs/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/jobs/tool-jobs/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/jobs/tool-jobs`. Defines `inject`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/jobs/jobs-local/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/jobs/jobs-local/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/jobs/jobs-local`. | `named-package-member` |
| [`packages/shell/shell-env/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/shell/shell-env/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/shell/shell-env`. | `named-package-member` |
| [`packages/shell/tool-bash/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/shell/tool-bash/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/shell/tool-bash`. | `named-package-member` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `user` | `const` | [`packages/boot/app-boot/src/index.ts:185`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/boot/app-boot/src/index.ts#L185) | `const user = home === resolve(cwd) ? undefined : readEnvLayer(binName, home, warn)` |
| `copy` | `const` | [`packages/client/ui-primitives/src/JsonTree.tsx:530`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-primitives/src/JsonTree.tsx#L530) | `const copy = async (mode: 'json' \| 'path' \| 'prettyJson' \| 'value') => {` |
| `standard` | `let` | [`packages/client/web-react/src/scoped-slots.tsx:364`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/web-react/src/scoped-slots.tsx#L364) | `let standard = byInfo.get(info)` |
| `list` | `const` | [`packages/hooks/hook-protocol/src/merge.ts:75`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/hooks/hook-protocol/src/merge.ts#L75) | `const list = reasonsByRank.get(r) ?? []` |
| `inject` | `const` | [`packages/jobs/tool-jobs/src/index.ts:22`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/jobs/tool-jobs/src/index.ts#L22) | `export const inject = ['tools', 'jobs', 'systemPrompt']` |
| `inject` | `const` | [`packages/jobs/tool-jobs/src/invariant.ts:15`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/jobs/tool-jobs/src/invariant.ts#L15) | `export const inject = ['invariants']` |
| `writableRoot` | `function` | [`packages/preset/agent-presets/src/authoring.ts:65`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/preset/agent-presets/src/authoring.ts#L65) | `export function writableRoot(roots: readonly PresetRoot[]): string {` |
| `broken` | `const` | [`packages/preset/agent-presets/src/discovery.ts:153`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/preset/agent-presets/src/discovery.ts#L153) | `const broken = await isFile(path)` |
| `AgentPresets` | `class` | [`packages/preset/agent-presets/src/index.ts:82`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/preset/agent-presets/src/index.ts#L82) | `export class AgentPresets extends Service {` |
| `inject` | `const` | [`packages/shell/shell-env/src/index.ts:26`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/shell/shell-env/src/index.ts#L26) | `export const inject: string[] = []` |
| `inject` | `const` | [`packages/shell/tool-bash/src/index.ts:31`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/shell/tool-bash/src/index.ts#L31) | `export const inject = ['tools', 'shell', 'systemPrompt', 'shellEnv']` |
| `files` | `const` | [`packages/typert/generator/src/analyzer.ts:220`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/typert/generator/src/analyzer.ts#L220) | `const files = new Map<string, ts.SourceFile \| undefined>()` |
| `row` | `const` | [`packages/web/tool-web/src/fetch.ts:62`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/web/tool-web/src/fetch.ts#L62) | `const row = cell.parentNode as HTMLTableRowElement` |
| `inject` | `const` | [`vendor/cordis/src/registry.ts:46`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/cordis/src/registry.ts#L46) | `const inject = (value[symbols.metadata] ??= {}).inject ??= Object.create(null)` |
| `resolve` | `function` | [`vendor/cordis/src/registry.ts:71`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/cordis/src/registry.ts#L71) | `export function resolve(inject: Inject \| null \| undefined, result: Dict = Object.create(null)) {` |

### Tests and executable evidence

- [`packages/jobs/jobs-local/tests/jobs.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/jobs/jobs-local/tests/jobs.spec.ts) — A test under the owning area exercises or imports `tool-jobs`. A test under the owning area exercises or imports `agentPresets`.
- [`packages/shell/tool-bash/tests/tools.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/shell/tool-bash/tests/tools.spec.ts) — A test under the owning area exercises or imports `run_in_background`.
- [`packages/jobs/tool-jobs/tests/tool-jobs.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/jobs/tool-jobs/tests/tool-jobs.spec.ts) — A test under the owning area exercises or imports `tool-jobs`. A test under the owning area exercises or imports `agentPresets`.
- [`packages/preset/agent-presets/tests/mount.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/preset/agent-presets/tests/mount.spec.ts) — A test under the owning area exercises or imports `AgentPresets`.
- [`packages/shell/tool-bash/tests/integration.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/shell/tool-bash/tests/integration.spec.ts) — A test under the owning area exercises or imports `tool-jobs`. A test under the owning area exercises or imports `run_in_background`.
- [`packages/preset/agent-presets/tests/settings.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/preset/agent-presets/tests/settings.spec.ts) — A test under the owning area exercises or imports `AgentPresets`.
- [`packages/preset/agent-presets/tests/authoring.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/preset/agent-presets/tests/authoring.spec.ts) — A test under the owning area exercises or imports `broken`. A test under the owning area exercises or imports `AgentPresets`.
- [`packages/preset/agent-presets/tests/user-root.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/preset/agent-presets/tests/user-root.spec.ts) — A test under the owning area exercises or imports `AgentPresets`. A test under the owning area exercises or imports `writableRoot`.

## How to read the implementation

1. Start with [`packages/skill/skill-filesystem/README.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/skill/skill-filesystem/README.md) because it has the strongest evidence link to the note.
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

- Tags: `class/bug-fix`, `concern/boundary`, `concern/discovery-routing`, `concern/evidence`, `concern/lifecycle`, `concern/ownership`, `concern/schema-types`, `concern/simplification`, `concern/trust`, `domain/build-release`, `domain/configuration`, `domain/extensions`, `domain/filesystem`, `domain/llm`, `domain/multi-agent`, `domain/security`, `domain/session-state`, `domain/shell-terminal`, `domain/testing`, `domain/tools`, `lifecycle/implemented`, `mechanism/generation`, `mechanism/registry`
- Aliases: `user`, `copy`, `standard`, `list`, `inject`, `writableRoot`, `broken`, `AgentPresets`, `files`, `row`, `resolve`, `editing-cordis-compositions`, `tool-bash`, `bashEnv`
- Regex: `(?i)(user|copy|standard|list|inject|writableRoot|broken|AgentPresets)`

```bash
rg -n --pcre2 "(?i)(user|copy|standard|list|inject|writableRoot|broken|AgentPresets)" source-deepseek-harness
rg -l --fixed-strings "class/bug-fix" 03-agent-context
```

## Connected notes

- **`source-link`** — [0112. Per-preset standing mounts over a scope parent chain](0112-per-preset-standing-mounts-over-a-scope-parent-chain.md): The source note links to this decision directly.
- **`source-link`** — [0355. Broken presets are roster rows, not gaps](0355-broken-presets-are-roster-rows-not-gaps.md): The source note links to this decision directly.
- **`source-link`** — [0478. Copy-only preset authoring, and the way into a preset's files](0478-copy-only-preset-authoring-and-the-way-into-a-preset-s-files.md): The source note links to this decision directly.
- **`shares-code-with`** — [0385. Generated tool-schema catalog (boot-and-harvest)](0385-generated-tool-schema-catalog-boot-and-harvest.md): Shares source implementation: `packages/jobs/tool-jobs/src/index.ts`, `packages/jobs/tool-jobs/src/invariant.ts`.
- **`shares-code-with`** — [0514. Cordis Host/Client Dynamic Plugin Runtime](0514-cordis-host-client-dynamic-plugin-runtime.md): Shares source implementation: `vendor/cordis/src/index.ts`, `vendor/cordis/src/registry.ts`.
- **`shares-code-with`** — [0121. Remote event delivery (ctx.remote.$on)](0121-remote-event-delivery-ctx-remote-on.md): Shares source implementation: `vendor/cordis/src/fiber.ts`, `vendor/cordis/src/index.ts`.
- **`shares-code-with`** — [0093. A session's agent is composed from a preset cordis.yml](0093-a-session-s-agent-is-composed-from-a-preset-cordis-yml.md): Shares source implementation: `vendor/cordis/src/fiber.ts`, `vendor/cordis/src/index.ts`.
- **`shares-code-with`** — [0015. The background job runtime (`ctx.jobs`) and generic task control tools](0015-the-background-job-runtime-ctx-jobs-and-generic-task-control-tools.md): Shares source implementation: `packages/jobs/jobs-local/src/index.ts`, `packages/jobs/tool-jobs/src/index.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0365-the-preset-authoring-agent-mount-validates-its-own-composition.md`.
