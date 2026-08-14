---
id: "dsh-note-0580"
title: "TUI skill slash command"
status: "archived"
class: "feature"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/archived/feature/2026-07-21-tui-skill-slash-command.md"
implementation_evidence: "medium"
target_anchor: "Rust provider-adapter boundary"
tags:
  - "class/feature"
  - "concern/boundary"
  - "concern/discovery-routing"
  - "concern/ownership"
  - "domain/build-release"
  - "domain/configuration"
  - "domain/llm"
  - "domain/session-state"
  - "domain/shell-terminal"
  - "domain/tools"
  - "domain/ui-interaction"
  - "lifecycle/archived"
  - "mechanism/policy"
  - "mechanism/registry"
aliases:
  - "surface"
  - "list"
  - "get"
  - "isUserInvocable"
  - "skill"
  - "skills"
  - "/skill:<name>"
  - "@deepseek-ai/dsh-tui"
  - "/skill:<name> [instructions]"
  - "agent.send"
  - "agent.steer"
  - "renderSkillInvocation"
  - "ctx.get"
  - "/skill:"
search_regex: "(?i)(surface|list|isUserInvocable|skill|skills|/skill:<name>|@deepseek\\-ai/dsh\\-tui|/skill:<name>[- ]\\[instructions\\])"
---

# 0580. TUI skill slash command — implementation context

## Open this when

The skill system shipped with model-initiated loading as its only path: the skill({ name }) tool lets the model pull a skill body into a turn, but a person driving the TUI could not load a skill on demand. Other coding agents expose a /skill: slash command for exactly this --- the user, not the model, decides a task matches a skill and injects its instructions. The skill-system note listed direct user invocation as deferred work, and the interactive front door is where it belongs.

## Source decision

The @deepseek-ai/dsh-tui front door owns a /skill: [instructions] command. On submit it loads the named skill and delivers one text block as a user turn --- sent with agent.send() while idle and agent.steer() while running, the same rule as ordinary editor input. The block is renderSkillInvocation(skill, instructions): a element wrapping the skill body, preceded by one resource-base line when the provider exposes one, with the user's trailing text appended after a blank line. The command is a TUI-only affordance; it adds no model-facing tool.

## Decision status

Historical design. Use it to understand an older pressure or failure; locate the replacing decision before implementing anything.

- Raw note: [archived/feature/2026-07-21-tui-skill-slash-command.md](../02-notes/archived/feature/2026-07-21-tui-skill-slash-command.md)
- Pinned source: [.agents/notes/archived/feature/2026-07-21-tui-skill-slash-command.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/archived/feature/2026-07-21-tui-skill-slash-command.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`packages/skill/skill/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/skill/skill/src/index.ts) | package entry point | Core file in the package named by the note: `packages/skill/skill`. Defines `skill`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/skill/skill/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/skill/skill/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/skill/skill`. | `named-package-member` |
| [`packages/skill/tool-skill/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/skill/tool-skill/src/index.ts) | package entry point | Core file in the package named by the note: `packages/skill/tool-skill`. Defines `skill`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/skill/tool-skill/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/skill/tool-skill/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/skill/tool-skill`. | `named-package-member` |
| [`packages/skill/skill`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/packages/skill/skill) | package or module directory | The note names this package or capability. | `named-package` |
| [`packages/skill/tool-skill`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/packages/skill/tool-skill) | package or module directory | The note names this package or capability. | `named-package` |
| [`packages/core/session/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/index.ts) | package entry point | Defines `surface`, a construct named by the note. | `symbol-definition` |
| [`packages/hooks/hook-protocol/src/merge.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/hooks/hook-protocol/src/merge.ts) | runtime implementation | Defines `list`, a construct named by the note. | `symbol-definition` |
| [`packages/shell/tool-bash-persistent/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/shell/tool-bash-persistent/src/index.ts) | package entry point | Defines `get`, a construct named by the note. | `symbol-definition` |
| [`packages/skill/skill/README.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/skill/skill/README.md) | package contract and examples | Core file in the package named by the note: `packages/skill/skill`. | `named-package-member` |
| [`packages/skill/skill/package.json`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/skill/skill/package.json) | composition and configuration | Core file in the package named by the note: `packages/skill/skill`. | `named-package-member` |
| [`packages/skill/tool-skill/README.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/skill/tool-skill/README.md) | package contract and examples | Core file in the package named by the note: `packages/skill/tool-skill`. | `named-package-member` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `surface` | `const` | [`packages/core/session/src/index.ts:727`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/index.ts#L727) | `const surface = this.surface` |
| `list` | `const` | [`packages/hooks/hook-protocol/src/merge.ts:75`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/hooks/hook-protocol/src/merge.ts#L75) | `const list = reasonsByRank.get(r) ?? []` |
| `get` | `const` | [`packages/shell/tool-bash-persistent/src/index.ts:227`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/shell/tool-bash-persistent/src/index.ts#L227) | `const get = (owner: Agent, signal: AbortSignal): Promise<TerminalSessionId> => {` |
| `isUserInvocable` | `function` | [`packages/skill/skill/src/index.ts:136`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/skill/skill/src/index.ts#L136) | `export function isUserInvocable(skill: Pick<SkillSummary, 'invocation'>): boolean {` |
| `skill` | `const` | [`packages/skill/skill/src/index.ts:574`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/skill/skill/src/index.ts#L574) | `const skill = entry.candidate` |
| `skill` | `const` | [`packages/skill/tool-skill/src/index.ts:141`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/skill/tool-skill/src/index.ts#L141) | `const skill = await ctx.skills.get(args.name, lookup)` |
| `skill` | `const` | [`packages/skill/tool-skill/src/index.ts:189`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/skill/tool-skill/src/index.ts#L189) | `const skill = await ctx.skills.get(name, lookup)` |
| `skills` | `const` | [`packages/skill/tool-skill/src/index.ts:226`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/skill/tool-skill/src/index.ts#L226) | `const skills = snapshot.skills.filter(isModelInvocable)` |

### Tests and executable evidence

- [`packages/skill/skill/tests/skill.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/skill/skill/tests/skill.spec.ts) — A test under the owning area exercises or imports `isUserInvocable`.
- [`packages/skill/tool-skill/tests/tool-skill.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/skill/tool-skill/tests/tool-skill.spec.ts) — A test under the owning area exercises or imports `dsh-tool-skill`. A test under the owning area exercises or imports `user-invocable`.

## How to read the implementation

1. Start with [`packages/skill/skill/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/skill/skill/src/index.ts) because it has the strongest evidence link to the note.
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
- **`rust/process-discipline`** — Treat this as repository or release policy unless it changes a runtime contract. Do not add runtime machinery for a process-only lesson.
- **`rust/provider-adapter`** — Keep provider wire quirks inside adapters; the loop and durable event vocabulary remain provider-neutral.
- **`rust/quickjs-boundary`** — QuickJS holds orchestration variables; Rust/Tokio owns async execution and exposes only explicitly granted capabilities.
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

- Tags: `class/feature`, `concern/boundary`, `concern/discovery-routing`, `concern/ownership`, `domain/build-release`, `domain/configuration`, `domain/llm`, `domain/session-state`, `domain/shell-terminal`, `domain/tools`, `domain/ui-interaction`, `lifecycle/archived`, `mechanism/policy`, `mechanism/registry`
- Aliases: `surface`, `list`, `get`, `isUserInvocable`, `skill`, `skills`, `/skill:<name>`, `@deepseek-ai/dsh-tui`, `/skill:<name> [instructions]`, `agent.send`, `agent.steer`, `renderSkillInvocation`, `ctx.get`, `/skill:`
- Regex: `(?i)(surface|list|isUserInvocable|skill|skills|/skill:<name>|@deepseek\-ai/dsh\-tui|/skill:<name>[- ]\[instructions\])`

```bash
rg -n --pcre2 "(?i)(surface|list|isUserInvocable|skill|skills|/skill:<name>|@deepseek\\-ai/dsh\\-tui|/skill:<name>[- ]\\[instructions\\])" source-deepseek-harness
rg -l --fixed-strings "class/feature" 03-agent-context
```

## Connected notes

- **`shares-code-with`** — [0192. Skill catalog hot refresh](0192-skill-catalog-hot-refresh.md): Shares source implementation: `packages/skill/skill`, `packages/skill/skill/src/index.ts`.
- **`shares-code-with`** — [0204. Independent model and user skill invocation policy](0204-independent-model-and-user-skill-invocation-policy.md): Shares source implementation: `packages/hooks/hook-protocol/src/merge.ts`, `packages/shell/tool-bash-persistent/src/index.ts`.
- **`shares-code-with`** — [0277. User-explicit skill invocation at the pre-step gesture boundary](0277-user-explicit-skill-invocation-at-the-pre-step-gesture-boundary.md): Shares source implementation: `packages/skill/skill`, `packages/skill/skill/src/index.ts`.
- **`shares-code-with`** — [0116. The skill registry is host-held and layered per scope](0116-the-skill-registry-is-host-held-and-layered-per-scope.md): Shares source implementation: `packages/skill/skill/src/index.ts`, `packages/skill/skill/src/invariant.ts`.
- **`shares-code-with`** — [0143. Skill system --- progressive disclosure instructions for agents](0143-skill-system-progressive-disclosure-instructions-for-agents.md): Shares source implementation: `packages/skill/skill/src/index.ts`, `packages/skill/skill/src/invariant.ts`.
- **`shares-code-with`** — [0271. Web skill tool row](0271-web-skill-tool-row.md): Shares source implementation: `packages/skill/skill`, `packages/skill/skill/src/index.ts`.
- **`shares-code-with`** — [0262. Bundled dsh badge skill](0262-bundled-dsh-badge-skill.md): Shares source implementation: `packages/skill/tool-skill`, `packages/skill/tool-skill/src/index.ts`.
- **`shares-code-with`** — [0549. Add direct directory listing to the filesystem seam](0549-add-direct-directory-listing-to-the-filesystem-seam.md): Shares source implementation: `packages/skill/skill`, `packages/skill/skill/src/index.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0580-tui-skill-slash-command.md`.
