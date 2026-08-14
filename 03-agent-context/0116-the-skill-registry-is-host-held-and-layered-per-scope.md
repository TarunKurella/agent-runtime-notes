---
id: "dsh-note-0116"
title: "The skill registry is host-held and layered per scope"
status: "implemented"
class: "architecture"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/architecture/2026-08-09-layered-skill-registry.md"
implementation_evidence: "medium"
target_anchor: "read/write/patch filesystem capability"
tags:
  - "class/architecture"
  - "concern/boundary"
  - "concern/discovery-routing"
  - "concern/ownership"
  - "domain/build-release"
  - "domain/configuration"
  - "domain/extensions"
  - "domain/filesystem"
  - "domain/llm"
  - "domain/protocols"
  - "domain/session-state"
  - "domain/testing"
  - "domain/ui-interaction"
  - "lifecycle/implemented"
  - "mechanism/generation"
  - "mechanism/registry"
aliases:
  - "scope"
  - "ScopedLayers"
  - "local"
  - "SkillLookupOptions"
  - "SkillViewOptions"
  - "SkillRegistry"
  - "skill"
  - "dsh-scope"
  - "ScopedLayers<SkillLayer>"
  - "registerProvider"
  - "skill-filesystem"
  - "tool-skill"
  - "serviceFor"
  - "The skill registry is host-held and layered per scope"
search_regex: "(?i)(scope|ScopedLayers|local|SkillLookupOptions|SkillViewOptions|SkillRegistry|skill|dsh\\-scope)"
---

# 0116. The skill registry is host-held and layered per scope — implementation context

## Open this when

The agent-preset stack moved the whole skill capability --- registry, local provider, and the skill tool --- into each preset's isolate realm, because "which skills an agent has" is an agent-plane choice. That framing conflated two different questions: which skills a deployment supplies, and whether an agent consumes them. A repository plugin's prepared wrapper declares inject: ['skills'] and mounts its skill root as a host-plane provider; with no host registry composed in the web and headless profiles, that wrapper waited forever and the repository-plugin e2e hung, which was bypassed at the time by dropping.

## Source decision

SkillRegistry adopts the same shape. It holds ScopedLayers; registerProvider() and register() file into the layer of the calling context's scope, so host rows and repository plugins land in the global layer while a preset's skill-filesystem --- mounted by the standing composition, whose context carries the preset's scope key --- lands in that preset's layer. Provider names are unique per layer rather than process-wide, which is what lets every preset mount its own local provider. Reads take the viewing scope through SkillViewOptions (the calling agent, which is its own scope key).

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/architecture/2026-08-09-layered-skill-registry.md](../02-notes/implemented/architecture/2026-08-09-layered-skill-registry.md)
- Pinned source: [.agents/notes/implemented/architecture/2026-08-09-layered-skill-registry.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/architecture/2026-08-09-layered-skill-registry.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`packages/core/scope/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/scope/src/index.ts) | package entry point | Core file in the package named by the note: `packages/core/scope`. Defines `scope`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/core/scope/src/store.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/scope/src/store.ts) | runtime implementation | Core file in the package named by the note: `packages/core/scope`. Defines `ScopedLayers`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/skill/skill/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/skill/skill/src/index.ts) | package entry point | Core file in the package named by the note: `packages/skill/skill`. Defines `skill`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/core/scope/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/scope/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/core/scope`. | `named-package-member` |
| [`packages/skill/skill/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/skill/skill/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/skill/skill`. | `named-package-member` |
| [`packages/skill/tool-skill/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/skill/tool-skill/src/index.ts) | package entry point | Core file in the package named by the note: `packages/skill/tool-skill`. Defines `skill`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/skill/tool-skill/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/skill/tool-skill/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/skill/tool-skill`. | `named-package-member` |
| [`packages/skill/skill-filesystem/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/skill/skill-filesystem/src/index.ts) | package entry point | Core file in the package named by the note: `packages/skill/skill-filesystem`. | `named-package-member` |
| [`packages/skill/skill-filesystem/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/skill/skill-filesystem/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/skill/skill-filesystem`. | `named-package-member` |
| [`packages/core/scope`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/packages/core/scope) | package or module directory | The note names this package or capability. | `named-package` |
| [`packages/skill/skill`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/packages/skill/skill) | package or module directory | The note names this package or capability. | `named-package` |
| [`packages/skill/tool-skill`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/packages/skill/tool-skill) | package or module directory | The note names this package or capability. | `named-package` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `scope` | `function` | [`packages/core/scope/src/index.ts:121`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/scope/src/index.ts#L121) | `function scope(): void {}` |
| `ScopedLayers` | `class` | [`packages/core/scope/src/store.ts:159`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/scope/src/store.ts#L159) | `export class ScopedLayers<L extends ScopeLayer> {` |
| `scope` | `const` | [`packages/core/scope/src/store.ts:231`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/scope/src/store.ts#L231) | `const scope = scopeOf(ctx)` |
| `local` | `const` | [`packages/fs/fs-local/src/index.ts:108`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/fs-local/src/index.ts#L108) | `const local = await resolveLocalTarget(opts?.cwd ?? this.config.cwd, path)` |
| `SkillLookupOptions` | `interface` | [`packages/skill/skill/src/index.ts:104`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/skill/skill/src/index.ts#L104) | `export interface SkillLookupOptions {` |
| `SkillViewOptions` | `interface` | [`packages/skill/skill/src/index.ts:117`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/skill/skill/src/index.ts#L117) | `export interface SkillViewOptions extends SkillLookupOptions {` |
| `SkillRegistry` | `class` | [`packages/skill/skill/src/index.ts:357`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/skill/skill/src/index.ts#L357) | `export class SkillRegistry extends Service {` |
| `scope` | `const` | [`packages/skill/skill/src/index.ts:442`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/skill/skill/src/index.ts#L442) | `const scope = scopeOf(this.ctx)` |
| `skill` | `const` | [`packages/skill/skill/src/index.ts:574`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/skill/skill/src/index.ts#L574) | `const skill = entry.candidate` |
| `skill` | `const` | [`packages/skill/tool-skill/src/index.ts:141`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/skill/tool-skill/src/index.ts#L141) | `const skill = await ctx.skills.get(args.name, lookup)` |
| `skill` | `const` | [`packages/skill/tool-skill/src/index.ts:189`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/skill/tool-skill/src/index.ts#L189) | `const skill = await ctx.skills.get(name, lookup)` |

### Tests and executable evidence

- [`packages/core/scope/tests/store.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/scope/tests/store.spec.ts) — A test under the owning area exercises or imports `ScopedLayers`.
- [`packages/skill/skill/tests/skill.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/skill/skill/tests/skill.spec.ts) — A test under the owning area exercises or imports `SkillRegistry`. A test under the owning area exercises or imports `SkillLookupOptions`.
- [`packages/skill/tool-skill/tests/tool-skill.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/skill/tool-skill/tests/tool-skill.spec.ts) — A test under the owning area exercises or imports `SkillRegistry`. A test under the owning area exercises or imports `registerProvider`.
- [`packages/skill/skill-filesystem/tests/skill-filesystem.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/skill/skill-filesystem/tests/skill-filesystem.spec.ts) — A test under the owning area exercises or imports `SkillRegistry`. A test under the owning area exercises or imports `registerProvider`.
- [`packages/skill/skill-filesystem/tests/skill-filesystem-watcher.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/skill/skill-filesystem/tests/skill-filesystem-watcher.spec.ts) — A test under the owning area exercises or imports `SkillRegistry`. A test under the owning area exercises or imports `skill-filesystem`.

## How to read the implementation

1. Start with [`packages/core/scope/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/scope/src/index.ts) because it has the strongest evidence link to the note.
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
- **`rust/process-discipline`** — Treat this as repository or release policy unless it changes a runtime contract. Do not add runtime machinery for a process-only lesson.
- **`rust/provider-adapter`** — Keep provider wire quirks inside adapters; the loop and durable event vocabulary remain provider-neutral.
- **`rust/quickjs-boundary`** — QuickJS holds orchestration variables; Rust/Tokio owns async execution and exposes only explicitly granted capabilities.
- **`rust/trait-seam`** — Use a small trait at the capability boundary; keep policy and model-facing schemas outside the backend implementation.

### Smallest safe implementation slice

1. Express the semantic contract as Rust types and one owning boundary.
2. Add the smallest adapter or service needed by the existing loop; do not add a new model-facing tool unless the four-tool surface cannot express the job.
3. Record durable evidence before deriving summaries or UI state.
4. Add one happy-path test, one interruption test, and one replay/restart test where state survives a turn.
5. Keep the patch reversible until the source note's strongest failure case is proven in the Rust harness.

## Search handles

- Tags: `class/architecture`, `concern/boundary`, `concern/discovery-routing`, `concern/ownership`, `domain/build-release`, `domain/configuration`, `domain/extensions`, `domain/filesystem`, `domain/llm`, `domain/protocols`, `domain/session-state`, `domain/testing`, `domain/ui-interaction`, `lifecycle/implemented`, `mechanism/generation`, `mechanism/registry`
- Aliases: `scope`, `ScopedLayers`, `local`, `SkillLookupOptions`, `SkillViewOptions`, `SkillRegistry`, `skill`, `dsh-scope`, `ScopedLayers<SkillLayer>`, `registerProvider`, `skill-filesystem`, `tool-skill`, `serviceFor`, `The skill registry is host-held and layered per scope`
- Regex: `(?i)(scope|ScopedLayers|local|SkillLookupOptions|SkillViewOptions|SkillRegistry|skill|dsh\-scope)`

```bash
rg -n --pcre2 "(?i)(scope|ScopedLayers|local|SkillLookupOptions|SkillViewOptions|SkillRegistry|skill|dsh\\-scope)" source-deepseek-harness
rg -l --fixed-strings "class/architecture" 03-agent-context
```

## Connected notes

- **`shares-code-with`** — [0192. Skill catalog hot refresh](0192-skill-catalog-hot-refresh.md): Shares source implementation: `packages/skill/skill-filesystem/src/index.ts`, `packages/skill/skill-filesystem/src/invariant.ts`.
- **`shares-code-with`** — [0201. Cross-workspace session resume](0201-cross-workspace-session-resume.md): Shares source implementation: `packages/core/scope`, `packages/core/scope/src/index.ts`.
- **`shares-code-with`** — [0277. User-explicit skill invocation at the pre-step gesture boundary](0277-user-explicit-skill-invocation-at-the-pre-step-gesture-boundary.md): Shares source implementation: `packages/skill/skill/src/index.ts`, `packages/skill/skill/src/invariant.ts`.
- **`shares-code-with`** — [0052. The slot system standard --- single register, four props shares, and the framework store seat](0052-the-slot-system-standard-single-register-four-props-shares-and-the-frame.md): Shares source implementation: `packages/core/scope`, `packages/core/scope/src/index.ts`.
- **`shares-code-with`** — [0580. TUI skill slash command](0580-tui-skill-slash-command.md): Shares source implementation: `packages/skill/skill/src/index.ts`, `packages/skill/skill/src/invariant.ts`.
- **`shares-code-with`** — [0262. Bundled dsh badge skill](0262-bundled-dsh-badge-skill.md): Shares source implementation: `packages/skill/skill-filesystem/src/index.ts`, `packages/skill/skill-filesystem/src/invariant.ts`.
- **`shares-code-with`** — [0143. Skill system --- progressive disclosure instructions for agents](0143-skill-system-progressive-disclosure-instructions-for-agents.md): Shares source implementation: `packages/skill/skill/src/index.ts`, `packages/skill/skill/src/invariant.ts`.
- **`shares-code-with`** — [0024. Prompt variables and tool-guidance ownership](0024-prompt-variables-and-tool-guidance-ownership.md): Shares source implementation: `packages/core/scope/src/index.ts`, `packages/core/scope/src/invariant.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0116-the-skill-registry-is-host-held-and-layered-per-scope.md`.
