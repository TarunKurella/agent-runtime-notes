---
id: "dsh-note-0355"
title: "Broken presets are roster rows, not gaps"
status: "implemented"
class: "bug-fix"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/bug-fix/2026-08-09-broken-preset-roster-rows.md"
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
  - "domain/security"
  - "domain/session-state"
  - "domain/shell-terminal"
  - "domain/testing"
  - "lifecycle/implemented"
aliases:
  - "presetOptions"
  - "copy"
  - "list"
  - "compositionProblem"
  - "scanRoot"
  - "broken"
  - "mount"
  - "PRESET_ID"
  - "AgentPreset"
  - "settings"
  - "resolve"
  - "entryListSchema"
  - "agent.cordis.yml"
  - "agentPreset.list"
search_regex: "(?i)(presetOptions|copy|list|compositionProblem|scanRoot|broken|mount|PRESET_ID)"
---

# 0355. Broken presets are roster rows, not gaps — implementation context

## Open this when

With files as the only composition editor, hand-edit damage had two failure shapes and both were silent until the worst moment. A preset whose agent.cordis.yml no longer parsed listed as a perfectly ordinary row --- selectable, copyable, settable as the default --- and failed only when the next session tried to mount it; set as default, every new session failed to start.

## Source decision

Discovery owns health, and a damaged directory is a roster row carrying a broken reason, never a gap. scanRoot treats every directory whose name is a usable preset id as a preset slot: composition missing → broken ("still occupies the id; delete it or restore the file"), composition unreadable/unparsable/not-a-list-of-named-rows → broken with the parser's first line. The shape check parses with the loader's own entryListSchema (the !!js dialect), so health can never call broken what the loader would accept; directories whose names fail PRESET_ID are skipped outright, because no copy could ever collide with them.

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/bug-fix/2026-08-09-broken-preset-roster-rows.md](../02-notes/implemented/bug-fix/2026-08-09-broken-preset-roster-rows.md)
- Pinned source: [.agents/notes/implemented/bug-fix/2026-08-09-broken-preset-roster-rows.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/bug-fix/2026-08-09-broken-preset-roster-rows.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`vendor/cordis/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/cordis/src/index.ts) | package entry point | Core file in the package named by the note: `vendor/cordis`. | `named-package-member` |
| [`vendor/cordis/src/fiber.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/cordis/src/fiber.ts) | runtime implementation | Core file in the package named by the note: `vendor/cordis`. | `named-package-member` |
| [`vendor/cordis/src/logger.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/cordis/src/logger.ts) | runtime implementation | Core file in the package named by the note: `vendor/cordis`. | `named-package-member` |
| [`vendor/cordis/src/registry.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/cordis/src/registry.ts) | runtime implementation | Core file in the package named by the note: `vendor/cordis`. Defines `resolve`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/settings/settings/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/settings/settings/src/index.ts) | package entry point | Core file in the package named by the note: `packages/settings/settings`. | `named-package-member` |
| [`packages/settings/settings/src/types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/settings/settings/src/types.ts) | public types and contract | Core file in the package named by the note: `packages/settings/settings`. | `named-package-member` |
| [`packages/settings/settings/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/settings/settings/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/settings/settings`. Defines `settings`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`vendor/cordis`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/vendor/cordis) | package or module directory | The note names this package or capability. | `named-package` |
| [`packages/settings/settings`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/packages/settings/settings) | package or module directory | The note names this package or capability. | `named-package` |
| [`vendor/include/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/include/src/index.ts) | package entry point | Defines `entryListSchema`, a construct named by the note. | `symbol-definition` |
| [`packages/hooks/hook-protocol/src/merge.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/hooks/hook-protocol/src/merge.ts) | runtime implementation | Defines `list`, a construct named by the note. | `symbol-definition` |
| [`packages/preset/agent-presets/src/mount.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/preset/agent-presets/src/mount.ts) | runtime implementation | Defines `mount`, a construct named by the note. | `symbol-definition` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `presetOptions` | `function` | [`packages/client/ui-agent-preset/src/client/settings-store.ts:153`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-agent-preset/src/client/settings-store.ts#L153) | `export function presetOptions(` |
| `copy` | `const` | [`packages/client/ui-primitives/src/JsonTree.tsx:530`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-primitives/src/JsonTree.tsx#L530) | `const copy = async (mode: 'json' \| 'path' \| 'prettyJson' \| 'value') => {` |
| `list` | `const` | [`packages/hooks/hook-protocol/src/merge.ts:75`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/hooks/hook-protocol/src/merge.ts#L75) | `const list = reasonsByRank.get(r) ?? []` |
| `compositionProblem` | `function` | [`packages/preset/agent-presets/src/discovery.ts:86`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/preset/agent-presets/src/discovery.ts#L86) | `async function compositionProblem(path: string): Promise<string \| undefined> {` |
| `scanRoot` | `function` | [`packages/preset/agent-presets/src/discovery.ts:139`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/preset/agent-presets/src/discovery.ts#L139) | `export async function scanRoot(root: PresetRoot): Promise<AgentPreset[]> {` |
| `broken` | `const` | [`packages/preset/agent-presets/src/discovery.ts:153`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/preset/agent-presets/src/discovery.ts#L153) | `const broken = await isFile(path)` |
| `mount` | `const` | [`packages/preset/agent-presets/src/mount.ts:261`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/preset/agent-presets/src/mount.ts#L261) | `const mount = standingMountFor(agent.ctx)` |
| `PRESET_ID` | `const` | [`packages/preset/agent-presets/src/preset.ts:18`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/preset/agent-presets/src/preset.ts#L18) | `export const PRESET_ID = /^[a-z0-9][a-z0-9-]*$/` |
| `AgentPreset` | `interface` | [`packages/preset/agent-presets/src/preset.ts:21`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/preset/agent-presets/src/preset.ts#L21) | `export interface AgentPreset {` |
| `settings` | `const` | [`packages/settings/settings/src/invariant.ts:25`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/settings/settings/src/invariant.ts#L25) | `const settings = ctx.get('settings')` |
| `resolve` | `function` | [`vendor/cordis/src/registry.ts:71`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/cordis/src/registry.ts#L71) | `export function resolve(inject: Inject \| null \| undefined, result: Dict = Object.create(null)) {` |
| `entryListSchema` | `const` | [`vendor/include/src/index.ts:23`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/include/src/index.ts#L23) | `export const entryListSchema = yaml.JSON_SCHEMA.extend(JsExpr)` |

### Tests and executable evidence

- [`packages/preset/agent-presets/tests/authoring.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/preset/agent-presets/tests/authoring.spec.ts) — A test under the owning area exercises or imports `broken`.
- [`packages/preset/agent-presets/tests/discovery.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/preset/agent-presets/tests/discovery.spec.ts) — A test under the owning area exercises or imports `scanRoot`.

## How to read the implementation

1. Start with [`vendor/cordis/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/cordis/src/index.ts) because it has the strongest evidence link to the note.
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

- Tags: `class/bug-fix`, `concern/boundary`, `concern/discovery-routing`, `concern/evidence`, `concern/lifecycle`, `concern/ownership`, `concern/schema-types`, `concern/simplification`, `concern/trust`, `domain/build-release`, `domain/configuration`, `domain/extensions`, `domain/filesystem`, `domain/security`, `domain/session-state`, `domain/shell-terminal`, `domain/testing`, `lifecycle/implemented`
- Aliases: `presetOptions`, `copy`, `list`, `compositionProblem`, `scanRoot`, `broken`, `mount`, `PRESET_ID`, `AgentPreset`, `settings`, `resolve`, `entryListSchema`, `agent.cordis.yml`, `agentPreset.list`
- Regex: `(?i)(presetOptions|copy|list|compositionProblem|scanRoot|broken|mount|PRESET_ID)`

```bash
rg -n --pcre2 "(?i)(presetOptions|copy|list|compositionProblem|scanRoot|broken|mount|PRESET_ID)" source-deepseek-harness
rg -l --fixed-strings "class/bug-fix" 03-agent-context
```

## Connected notes

- **`source-link`** — [0365. The preset-authoring agent mount-validates its own composition](0365-the-preset-authoring-agent-mount-validates-its-own-composition.md): The source note links to this decision directly.
- **`shares-code-with`** — [0478. Copy-only preset authoring, and the way into a preset's files](0478-copy-only-preset-authoring-and-the-way-into-a-preset-s-files.md): Shares source implementation: `vendor/cordis`, `vendor/cordis/src/fiber.ts`.
- **`shares-code-with`** — [0086. settings write-path integrity and observer lifecycle](0086-settings-write-path-integrity-and-observer-lifecycle.md): Shares source implementation: `packages/settings/settings`, `packages/settings/settings/src/index.ts`.
- **`shares-code-with`** — [0121. Remote event delivery (ctx.remote.$on)](0121-remote-event-delivery-ctx-remote-on.md): Shares source implementation: `vendor/cordis/src/fiber.ts`, `vendor/cordis/src/index.ts`.
- **`shares-code-with`** — [0093. A session's agent is composed from a preset cordis.yml](0093-a-session-s-agent-is-composed-from-a-preset-cordis-yml.md): Shares source implementation: `vendor/cordis/src/fiber.ts`, `vendor/cordis/src/index.ts`.
- **`shares-code-with`** — [0514. Cordis Host/Client Dynamic Plugin Runtime](0514-cordis-host-client-dynamic-plugin-runtime.md): Shares source implementation: `vendor/cordis/src/index.ts`, `vendor/cordis/src/logger.ts`.
- **`shares-code-with`** — [0597. `dsh meta` boots the TUI over the harness checkout](0597-dsh-meta-boots-the-tui-over-the-harness-checkout.md): Shares source implementation: `vendor/cordis/src/fiber.ts`, `vendor/cordis/src/index.ts`.
- **`shares-code-with`** — [0073. user-settings seam (`ctx.settings`) and the file provider](0073-user-settings-seam-ctx-settings-and-the-file-provider.md): Shares source implementation: `packages/settings/settings/src/index.ts`, `packages/settings/settings/src/invariant.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0355-broken-presets-are-roster-rows-not-gaps.md`.
