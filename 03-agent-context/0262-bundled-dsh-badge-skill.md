---
id: "dsh-note-0262"
title: "Bundled dsh badge skill"
status: "implemented"
class: "feature"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/feature/2026-08-06-bundled-dsh-badge-skill.md"
implementation_evidence: "medium"
target_anchor: "read/write/patch filesystem capability"
tags:
  - "class/feature"
  - "concern/boundary"
  - "concern/discovery-routing"
  - "concern/lifecycle"
  - "concern/ownership"
  - "domain/configuration"
  - "domain/extensions"
  - "domain/filesystem"
  - "domain/llm"
  - "domain/storage"
  - "lifecycle/implemented"
  - "mechanism/projection"
  - "mechanism/registry"
aliases:
  - "skills"
  - "@deepseek-ai/dsh-skill-badge"
  - "ctx.skills"
  - "dsh-badge"
  - "dsh-tool-skill"
  - "skill-badge"
  - "dsh-skill-filesystem"
  - "Bundled dsh badge skill"
  - "feature"
  - "boundary"
  - "discovery routing"
  - "lifecycle"
  - "ownership"
  - "configuration"
search_regex: "(?i)(skills|@deepseek\\-ai/dsh\\-skill\\-badge|ctx\\.skills|dsh\\-badge|dsh\\-tool\\-skill|skill\\-badge|dsh\\-skill\\-filesystem|Bundled[- ]dsh[- ]badge[- ]skill)"
---

# 0262. Bundled dsh badge skill — implementation context

## Open this when

The Cordis tutorial uses an official "powered by dsh" badge across its pages, but the shipped CLI has no reusable instructions or explicit opt-in provider for applying the same attribution elsewhere.

## Source decision

@deepseek-ai/dsh-skill-badge is a native Cordis plugin that registers one immutable bundled provider on ctx.skills. The provider owns the dsh-badge summary, instruction body, and PNG resource base; dsh-tool-skill remains the sole owner of model-facing catalog and loader rendering. The shipped CLI composition declares skill-badge as disabled. Enabling that existing row is the explicit opt-in; disabled installations advertise no badge skill and gain no model-visible content.

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/feature/2026-08-06-bundled-dsh-badge-skill.md](../02-notes/implemented/feature/2026-08-06-bundled-dsh-badge-skill.md)
- Pinned source: [.agents/notes/implemented/feature/2026-08-06-bundled-dsh-badge-skill.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/feature/2026-08-06-bundled-dsh-badge-skill.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`docs/cordis-tutorial/index.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/cordis-tutorial/index.md) | package contract and examples | The source note names this file directly. | `named-file` |
| [`packages/skill/tool-skill/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/skill/tool-skill/src/index.ts) | package entry point | Core file in the package named by the note: `packages/skill/tool-skill`. Defines `skills`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/skill/skill-badge/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/skill/skill-badge/src/index.ts) | package entry point | Core file in the package named by the note: `packages/skill/skill-badge`. Contains the exact code literal `dsh-badge` named by the note. | `exact-code-occurrence, named-package-member` |
| [`packages/skill/tool-skill/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/skill/tool-skill/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/skill/tool-skill`. | `named-package-member` |
| [`packages/skill/skill-badge/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/skill/skill-badge/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/skill/skill-badge`. | `named-package-member` |
| [`packages/skill/skill-filesystem/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/skill/skill-filesystem/src/index.ts) | package entry point | Core file in the package named by the note: `packages/skill/skill-filesystem`. Defines `skills`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/skill/skill-filesystem/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/skill/skill-filesystem/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/skill/skill-filesystem`. | `named-package-member` |
| [`packages/skill/tool-skill`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/packages/skill/tool-skill) | package or module directory | The note names this package or capability. | `named-package` |
| [`packages/skill/skill-badge`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/packages/skill/skill-badge) | package or module directory | The note names this package or capability. | `named-package` |
| [`packages/skill/skill-filesystem`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/packages/skill/skill-filesystem) | package or module directory | The note names this package or capability. | `named-package` |
| [`packages/skill/tool-skill/README.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/skill/tool-skill/README.md) | package contract and examples | Core file in the package named by the note: `packages/skill/tool-skill`. | `named-package-member` |
| [`packages/skill/skill-badge/README.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/skill/skill-badge/README.md) | package contract and examples | Core file in the package named by the note: `packages/skill/skill-badge`. Contains the exact code literal `dsh-badge` named by the note. | `exact-code-occurrence, named-package-member` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `skills` | `const` | [`packages/skill/skill-filesystem/src/index.ts:720`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/skill/skill-filesystem/src/index.ts#L720) | `const skills: SkillCandidate[] = []` |
| `skills` | `const` | [`packages/skill/tool-skill/src/index.ts:226`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/skill/tool-skill/src/index.ts#L226) | `const skills = snapshot.skills.filter(isModelInvocable)` |

### Tests and executable evidence

- [`packages/skill/tool-skill/tests/tool-skill.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/skill/tool-skill/tests/tool-skill.spec.ts) — A test under the owning area exercises or imports `dsh-tool-skill`. A test under the owning area exercises or imports `dsh-skill-filesystem`.
- [`packages/skill/skill-badge/tests/skill-badge.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/skill/skill-badge/tests/skill-badge.spec.ts) — A test under the owning area exercises or imports `dsh-badge`. Contains the exact code literal `dsh-badge` named by the note.
- [`packages/skill/skill-filesystem/tests/skill-filesystem.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/skill/skill-filesystem/tests/skill-filesystem.spec.ts) — A test under the owning area exercises or imports `dsh-skill-filesystem`.
- [`apps/cli/tests/dsh-badge.snapshot.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/cli/tests/dsh-badge.snapshot.ts) — Contains the exact code literal `dsh-badge` named by the note.
- [`apps/cli/tests/web-agent-presets.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/cli/tests/web-agent-presets.e2e.ts) — Contains the exact code literal `dsh-badge` named by the note.
- [`apps/cli/tests/fixtures/dsh-badge/snapshot.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/cli/tests/fixtures/dsh-badge/snapshot.ts) — Contains the exact code literal `dsh-badge` named by the note.
- [`apps/web/tests/goal-multi-turn-actions.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/goal-multi-turn-actions.e2e.ts) — Contains the exact code literal `dsh-skill-filesystem` named by the note.

## How to read the implementation

1. Start with [`docs/cordis-tutorial/index.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/cordis-tutorial/index.md) because it has the strongest evidence link to the note.
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

- Tags: `class/feature`, `concern/boundary`, `concern/discovery-routing`, `concern/lifecycle`, `concern/ownership`, `domain/configuration`, `domain/extensions`, `domain/filesystem`, `domain/llm`, `domain/storage`, `lifecycle/implemented`, `mechanism/projection`, `mechanism/registry`
- Aliases: `skills`, `@deepseek-ai/dsh-skill-badge`, `ctx.skills`, `dsh-badge`, `dsh-tool-skill`, `skill-badge`, `dsh-skill-filesystem`, `Bundled dsh badge skill`, `feature`, `boundary`, `discovery routing`, `lifecycle`, `ownership`, `configuration`
- Regex: `(?i)(skills|@deepseek\-ai/dsh\-skill\-badge|ctx\.skills|dsh\-badge|dsh\-tool\-skill|skill\-badge|dsh\-skill\-filesystem|Bundled[- ]dsh[- ]badge[- ]skill)`

```bash
rg -n --pcre2 "(?i)(skills|@deepseek\\-ai/dsh\\-skill\\-badge|ctx\\.skills|dsh\\-badge|dsh\\-tool\\-skill|skill\\-badge|dsh\\-skill\\-filesystem|Bundled[- ]dsh[- ]badge[- ]skill)" source-deepseek-harness
rg -l --fixed-strings "class/feature" 03-agent-context
```

## Connected notes

- **`shares-code-with`** — [0192. Skill catalog hot refresh](0192-skill-catalog-hot-refresh.md): Shares source implementation: `packages/skill/skill-filesystem/src/index.ts`, `packages/skill/skill-filesystem/src/invariant.ts`.
- **`shares-code-with`** — [0116. The skill registry is host-held and layered per scope](0116-the-skill-registry-is-host-held-and-layered-per-scope.md): Shares source implementation: `packages/skill/skill-filesystem/src/index.ts`, `packages/skill/skill-filesystem/src/invariant.ts`.
- **`shares-code-with`** — [0277. User-explicit skill invocation at the pre-step gesture boundary](0277-user-explicit-skill-invocation-at-the-pre-step-gesture-boundary.md): Shares source implementation: `packages/skill/tool-skill`, `packages/skill/tool-skill/src/index.ts`.
- **`shares-code-with`** — [0483. Remove the dedicated repository Plugin path](0483-remove-the-dedicated-repository-plugin-path.md): Shares source implementation: `packages/skill/skill-filesystem`, `packages/skill/skill-filesystem/src/index.ts`.
- **`shares-code-with`** — [0204. Independent model and user skill invocation policy](0204-independent-model-and-user-skill-invocation-policy.md): Shares source implementation: `packages/skill/tool-skill`, `packages/skill/tool-skill/src/index.ts`.
- **`shares-code-with`** — [0580. TUI skill slash command](0580-tui-skill-slash-command.md): Shares source implementation: `packages/skill/tool-skill`, `packages/skill/tool-skill/src/index.ts`.
- **`shares-code-with`** — [0143. Skill system --- progressive disclosure instructions for agents](0143-skill-system-progressive-disclosure-instructions-for-agents.md): Shares source implementation: `packages/skill/skill-badge/src/index.ts`, `packages/skill/tool-skill/src/index.ts`.
- **`shares-code-with`** — [0256. Producer-declared context forms](0256-producer-declared-context-forms.md): Shares source implementation: `packages/skill/tool-skill/src/index.ts`, `packages/skill/tool-skill/src/invariant.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0262-bundled-dsh-badge-skill.md`.
