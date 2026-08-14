---
id: "dsh-note-0277"
title: "User-explicit skill invocation at the pre-step gesture boundary"
status: "implemented"
class: "feature"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/feature/2026-08-08-user-explicit-skill-invocation.md"
implementation_evidence: "medium"
target_anchor: "read/write/patch filesystem capability"
tags:
  - "class/feature"
  - "concern/boundary"
  - "concern/concurrency"
  - "concern/discovery-routing"
  - "concern/schema-types"
  - "domain/agent-loop"
  - "domain/context"
  - "domain/extensions"
  - "domain/filesystem"
  - "domain/llm"
  - "domain/observability"
  - "domain/protocols"
  - "domain/session-state"
  - "domain/storage"
  - "domain/testing"
  - "domain/tools"
  - "domain/ui-interaction"
  - "lifecycle/implemented"
  - "mechanism/event-sourcing"
  - "mechanism/policy"
  - "mechanism/projection"
  - "mechanism/registry"
aliases:
  - "user"
  - "list"
  - "prompt"
  - "get"
  - "isUserInvocable"
  - "renderSkillContent"
  - "skill"
  - "disable-model-invocation: true"
  - "skill.list"
  - "/name"
  - "dsh-tool-skill"
  - "agent/pre-step"
  - "agent-instructions"
  - "ctx.skills.get"
search_regex: "(?i)(user|list|prompt|isUserInvocable|renderSkillContent|skill|disable\\-model\\-invocation:[- ]true|skill\\.list)"
---

# 0277. User-explicit skill invocation at the pre-step gesture boundary — implementation context

## Open this when

A disable-model-invocation: true skill is user-only by design: it never enters the model-facing catalog and the skill tool refuses to load it. Its only legitimate entry point is an explicit user gesture --- yet the web client had none. skill.list filtered to the model-and-user intersection (hiding user-only skills from the menu), an entered /name line rode into the default prompt sink as plain text, and the model it reached was forbidden to load the skill --- so it degraded to read-ing the SKILL.md file or ignoring the gesture (issue #1470).

## Source decision

User-explicit invocation is a host-side pre-step injection, uniform for every user-invocable skill and every entry point: dsh-tool-skill registers a second agent/pre-step listener (beside its catalog listener, the same seam agent-instructions and the runtime-context snapshot ride): it scans the step's claimed messages for whitespace-bounded /name tokens --- anywhere in the text, the same word-boundary shape the transcript chip decoration uses --- collects first-seen-deduplicated names, loads each through ctx.skills.get, checks isUserInvocable on the loaded definition (the single lookup that produces what is.

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/feature/2026-08-08-user-explicit-skill-invocation.md](../02-notes/implemented/feature/2026-08-08-user-explicit-skill-invocation.md)
- Pinned source: [.agents/notes/implemented/feature/2026-08-08-user-explicit-skill-invocation.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/feature/2026-08-08-user-explicit-skill-invocation.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`packages/skill/skill/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/skill/skill/src/index.ts) | package entry point | Core file in the package named by the note: `packages/skill/skill`. Defines `skill`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/skill/skill/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/skill/skill/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/skill/skill`. | `named-package-member` |
| [`packages/skill/tool-skill/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/skill/tool-skill/src/index.ts) | package entry point | Core file in the package named by the note: `packages/skill/tool-skill`. Defines `skill`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/skill/tool-skill/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/skill/tool-skill/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/skill/tool-skill`. | `named-package-member` |
| [`packages/context/agent-instructions/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/context/agent-instructions/src/index.ts) | package entry point | Core file in the package named by the note: `packages/context/agent-instructions`. | `named-package-member` |
| [`packages/context/agent-instructions/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/context/agent-instructions/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/context/agent-instructions`. | `named-package-member` |
| [`packages/skill/skill`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/packages/skill/skill) | package or module directory | The note names this package or capability. | `named-package` |
| [`packages/skill/tool-skill`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/packages/skill/tool-skill) | package or module directory | The note names this package or capability. | `named-package` |
| [`packages/context/agent-instructions`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/packages/context/agent-instructions) | package or module directory | The note names this package or capability. | `named-package` |
| [`packages/boot/app-boot/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/boot/app-boot/src/index.ts) | package entry point | Defines `user`, a construct named by the note. | `symbol-definition` |
| [`packages/schedule/schedule/src/domain.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/schedule/schedule/src/domain.ts) | runtime implementation | Defines `prompt`, a construct named by the note. | `symbol-definition` |
| [`packages/hooks/hook-protocol/src/merge.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/hooks/hook-protocol/src/merge.ts) | runtime implementation | Defines `list`, a construct named by the note. | `symbol-definition` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `user` | `const` | [`packages/boot/app-boot/src/index.ts:185`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/boot/app-boot/src/index.ts#L185) | `const user = home === resolve(cwd) ? undefined : readEnvLayer(binName, home, warn)` |
| `list` | `const` | [`packages/hooks/hook-protocol/src/merge.ts:75`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/hooks/hook-protocol/src/merge.ts#L75) | `const list = reasonsByRank.get(r) ?? []` |
| `prompt` | `const` | [`packages/schedule/schedule/src/domain.ts:389`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/schedule/schedule/src/domain.ts#L389) | `const prompt = value['prompt']` |
| `get` | `const` | [`packages/shell/tool-bash-persistent/src/index.ts:227`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/shell/tool-bash-persistent/src/index.ts#L227) | `const get = (owner: Agent, signal: AbortSignal): Promise<TerminalSessionId> => {` |
| `isUserInvocable` | `function` | [`packages/skill/skill/src/index.ts:136`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/skill/skill/src/index.ts#L136) | `export function isUserInvocable(skill: Pick<SkillSummary, 'invocation'>): boolean {` |
| `renderSkillContent` | `function` | [`packages/skill/skill/src/index.ts:171`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/skill/skill/src/index.ts#L171) | `export function renderSkillContent(skill: Pick<SkillDefinition, 'name' \| 'provider' \| 'resourceBase' \| 'content'>): string {` |
| `skill` | `const` | [`packages/skill/skill/src/index.ts:574`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/skill/skill/src/index.ts#L574) | `const skill = entry.candidate` |
| `skill` | `const` | [`packages/skill/tool-skill/src/index.ts:141`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/skill/tool-skill/src/index.ts#L141) | `const skill = await ctx.skills.get(args.name, lookup)` |
| `skill` | `const` | [`packages/skill/tool-skill/src/index.ts:189`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/skill/tool-skill/src/index.ts#L189) | `const skill = await ctx.skills.get(name, lookup)` |

### Tests and executable evidence

- [`packages/skill/skill/tests/skill.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/skill/skill/tests/skill.spec.ts) — A test under the owning area exercises or imports `isUserInvocable`. A test under the owning area exercises or imports `renderSkillContent`.
- [`packages/skill/tool-skill/tests/tool-skill.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/skill/tool-skill/tests/tool-skill.spec.ts) — A test under the owning area exercises or imports `dsh-tool-skill`. A test under the owning area exercises or imports `modelInvocable`.
- [`packages/context/agent-instructions/tests/agent-instructions.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/context/agent-instructions/tests/agent-instructions.e2e.ts) — A test under the owning area exercises or imports `agent-instructions`.
- [`packages/context/agent-instructions/tests/agent-instructions.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/context/agent-instructions/tests/agent-instructions.spec.ts) — A test under the owning area exercises or imports `agent-instructions`.
- [`apps/web/tests/scaffold-hermetic.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/scaffold-hermetic.e2e.ts) — Contains the exact code literal `dsh-skill` named by the note.
- [`apps/cli/tests/fixtures/dsh-badge/snapshot.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/cli/tests/fixtures/dsh-badge/snapshot.ts) — Contains the exact code literal `agent/pre-step` named by the note.

## How to read the implementation

1. Start with [`packages/skill/skill/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/skill/skill/src/index.ts) because it has the strongest evidence link to the note.
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
- **`rust/context-evidence`** — Compaction may shrink presentation, but it must not erase the evidence ledger, file activity, or durable session facts.
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

- Tags: `class/feature`, `concern/boundary`, `concern/concurrency`, `concern/discovery-routing`, `concern/schema-types`, `domain/agent-loop`, `domain/context`, `domain/extensions`, `domain/filesystem`, `domain/llm`, `domain/observability`, `domain/protocols`, `domain/session-state`, `domain/storage`, `domain/testing`, `domain/tools`, `domain/ui-interaction`, `lifecycle/implemented`, `mechanism/event-sourcing`, `mechanism/policy`, `mechanism/projection`, `mechanism/registry`
- Aliases: `user`, `list`, `prompt`, `get`, `isUserInvocable`, `renderSkillContent`, `skill`, `disable-model-invocation: true`, `skill.list`, `/name`, `dsh-tool-skill`, `agent/pre-step`, `agent-instructions`, `ctx.skills.get`
- Regex: `(?i)(user|list|prompt|isUserInvocable|renderSkillContent|skill|disable\-model\-invocation:[- ]true|skill\.list)`

```bash
rg -n --pcre2 "(?i)(user|list|prompt|isUserInvocable|renderSkillContent|skill|disable\\-model\\-invocation:[- ]true|skill\\.list)" source-deepseek-harness
rg -l --fixed-strings "class/feature" 03-agent-context
```

## Connected notes

- **`source-link`** — [0063. Web input state machine, composer slots, and the slash pipeline (ui-conversation input / ui-input-trigger)](0063-web-input-state-machine-composer-slots-and-the-slash-pipeline-ui-convers.md): The source note links to this decision directly.
- **`shares-code-with`** — [0192. Skill catalog hot refresh](0192-skill-catalog-hot-refresh.md): Shares source implementation: `packages/skill/skill`, `packages/skill/skill/src/index.ts`.
- **`shares-code-with`** — [0580. TUI skill slash command](0580-tui-skill-slash-command.md): Shares source implementation: `packages/skill/skill`, `packages/skill/skill/src/index.ts`.
- **`shares-code-with`** — [0252. Web transcript marks context source, recall, and steering](0252-web-transcript-marks-context-source-recall-and-steering.md): Shares source implementation: `packages/context/agent-instructions/src/index.ts`, `packages/context/agent-instructions/src/invariant.ts`.
- **`shares-code-with`** — [0204. Independent model and user skill invocation policy](0204-independent-model-and-user-skill-invocation-policy.md): Shares source implementation: `packages/skill/skill/src/index.ts`, `packages/skill/tool-skill`.
- **`shares-code-with`** — [0221. Read card --- the read tool's structured line window reaches the client](0221-read-card-the-read-tool-s-structured-line-window-reaches-the-client.md): Shares source implementation: `packages/context/agent-instructions`, `packages/context/agent-instructions/src/index.ts`.
- **`shares-code-with`** — [0116. The skill registry is host-held and layered per scope](0116-the-skill-registry-is-host-held-and-layered-per-scope.md): Shares source implementation: `packages/skill/skill/src/index.ts`, `packages/skill/skill/src/invariant.ts`.
- **`shares-code-with`** — [0143. Skill system --- progressive disclosure instructions for agents](0143-skill-system-progressive-disclosure-instructions-for-agents.md): Shares source implementation: `packages/skill/skill/src/index.ts`, `packages/skill/skill/src/invariant.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0277-user-explicit-skill-invocation-at-the-pre-step-gesture-boundary.md`.
