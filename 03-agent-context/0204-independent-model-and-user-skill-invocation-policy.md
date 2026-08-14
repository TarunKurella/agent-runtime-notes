---
id: "dsh-note-0204"
title: "Independent model and user skill invocation policy"
status: "implemented"
class: "feature"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/feature/2026-07-28-skill-invocation-policy.md"
implementation_evidence: "medium"
target_anchor: "sandbox capability boundary"
tags:
  - "class/feature"
  - "concern/boundary"
  - "concern/compatibility"
  - "concern/discovery-routing"
  - "concern/evidence"
  - "concern/human-control"
  - "concern/ownership"
  - "concern/schema-types"
  - "concern/trust"
  - "domain/build-release"
  - "domain/configuration"
  - "domain/llm"
  - "domain/observability"
  - "domain/protocols"
  - "domain/security"
  - "domain/shell-terminal"
  - "domain/storage"
  - "domain/ui-interaction"
  - "domain/web-retrieval"
  - "lifecycle/implemented"
  - "mechanism/policy"
  - "mechanism/registry"
aliases:
  - "off"
  - "list"
  - "metadata"
  - "get"
  - "SkillSummary"
  - "SkillRegistration"
  - "isModelInvocable"
  - "isUserInvocable"
  - "whenToUse"
  - "ctx.skills.list"
  - "ctx.skills.get"
  - "disable-model-invocation"
  - "user-invocable"
  - "invocation: SkillInvocationPolicy"
search_regex: "(?i)(list|metadata|SkillSummary|SkillRegistration|isModelInvocable|isUserInvocable|whenToUse|ctx\\.skills\\.list)"
---

# 0204. Independent model and user skill invocation policy — implementation context

## Open this when

The skill registry originally treated discovery as a model catalog: ctx.skills.list() removed model-disabled skills, while ctx.skills.get() remained an unfiltered trusted loader. That was enough for model-initiated loading, but it could not represent Claude-compatible skills that are advertised only to a person, only to a model, to both, or to neither. The TUI compounded the mismatch by deriving user autocomplete from the model-filtered list and allowing every exact name through get(). The local parser also exposed an internal camel-case spelling as frontmatter.

## Source decision

SkillSummary carries a required typed invocation: SkillInvocationPolicy object whose modelInvocable: boolean and userInvocable: boolean fields are positive and symmetric. Omission exists only at explicit input boundaries: a runtime SkillRegistration without a policy and local frontmatter without either invocation key resolve to { modelInvocable: true, userInvocable: true } before producing candidates or definitions.

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/feature/2026-07-28-skill-invocation-policy.md](../02-notes/implemented/feature/2026-07-28-skill-invocation-policy.md)
- Pinned source: [.agents/notes/implemented/feature/2026-07-28-skill-invocation-policy.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/feature/2026-07-28-skill-invocation-policy.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`packages/skill/tool-skill/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/skill/tool-skill/src/index.ts) | package entry point | Core file in the package named by the note: `packages/skill/tool-skill`. | `named-package-member` |
| [`packages/skill/tool-skill/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/skill/tool-skill/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/skill/tool-skill`. | `named-package-member` |
| [`packages/skill/tool-skill`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/packages/skill/tool-skill) | package or module directory | The note names this package or capability. | `named-package` |
| [`packages/skill/skill/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/skill/skill/src/index.ts) | package entry point | Defines `SkillSummary`, a construct named by the note. Defines `SkillRegistration`, a construct named by the note. | `symbol-definition` |
| [`packages/host/apiproxy/src/api-proxy.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/src/api-proxy.ts) | runtime implementation | Defines `metadata`, a construct named by the note. | `symbol-definition` |
| [`packages/hooks/hook-protocol/src/merge.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/hooks/hook-protocol/src/merge.ts) | runtime implementation | Defines `list`, a construct named by the note. | `symbol-definition` |
| [`packages/client/ui-layout/src/client/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-layout/src/client/index.ts) | package entry point | Defines `off`, a construct named by the note. | `symbol-definition` |
| [`packages/shell/tool-bash-persistent/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/shell/tool-bash-persistent/src/index.ts) | package entry point | Defines `get`, a construct named by the note. | `symbol-definition` |
| [`packages/skill/tool-skill/README.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/skill/tool-skill/README.md) | package contract and examples | Core file in the package named by the note: `packages/skill/tool-skill`. | `named-package-member` |
| [`packages/skill/tool-skill/package.json`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/skill/tool-skill/package.json) | composition and configuration | Core file in the package named by the note: `packages/skill/tool-skill`. | `named-package-member` |
| [`pnpm-lock.yaml`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/pnpm-lock.yaml) | composition and configuration | Contains the exact code literal `dsh-tool-skill` named by the note. | `exact-code-occurrence` |
| [`docs/tool-catalog.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/tool-catalog.md) | package contract and examples | Contains the exact code literal `dsh-tool-skill` named by the note. | `exact-code-occurrence` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `off` | `const` | [`packages/client/ui-layout/src/client/index.ts:150`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-layout/src/client/index.ts#L150) | `const off = ctx.on('theme/change', (snapshot) => { presenter.apply(snapshot) })` |
| `list` | `const` | [`packages/hooks/hook-protocol/src/merge.ts:75`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/hooks/hook-protocol/src/merge.ts#L75) | `const list = reasonsByRank.get(r) ?? []` |
| `metadata` | `const` | [`packages/host/apiproxy/src/api-proxy.ts:555`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/src/api-proxy.ts#L555) | `const metadata = sessionListMetadata(session.events)` |
| `get` | `const` | [`packages/shell/tool-bash-persistent/src/index.ts:227`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/shell/tool-bash-persistent/src/index.ts#L227) | `const get = (owner: Agent, signal: AbortSignal): Promise<TerminalSessionId> => {` |
| `SkillSummary` | `interface` | [`packages/skill/skill/src/index.ts:56`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/skill/skill/src/index.ts#L56) | `export interface SkillSummary {` |
| `SkillRegistration` | `type` | [`packages/skill/skill/src/index.ts:96`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/skill/skill/src/index.ts#L96) | `export type SkillRegistration = Omit<SkillDefinition, 'invocation' \| 'provider'> & {` |
| `isModelInvocable` | `function` | [`packages/skill/skill/src/index.ts:127`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/skill/skill/src/index.ts#L127) | `export function isModelInvocable(skill: Pick<SkillSummary, 'invocation'>): boolean {` |
| `isUserInvocable` | `function` | [`packages/skill/skill/src/index.ts:136`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/skill/skill/src/index.ts#L136) | `export function isUserInvocable(skill: Pick<SkillSummary, 'invocation'>): boolean {` |
| `whenToUse` | `const` | [`packages/skill/skill/src/index.ts:752`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/skill/skill/src/index.ts#L752) | `const whenToUse = skill.whenToUse` |

### Tests and executable evidence

- [`packages/skill/skill/tests/skill.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/skill/skill/tests/skill.spec.ts) — A test under the owning area exercises or imports `isModelInvocable`. A test under the owning area exercises or imports `isUserInvocable`.
- [`packages/skill/tool-skill/tests/tool-skill.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/skill/tool-skill/tests/tool-skill.spec.ts) — A test under the owning area exercises or imports `disable-model-invocation`. A test under the owning area exercises or imports `user-invocable`.

## How to read the implementation

1. Start with [`packages/skill/tool-skill/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/skill/tool-skill/src/index.ts) because it has the strongest evidence link to the note.
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

- Tags: `class/feature`, `concern/boundary`, `concern/compatibility`, `concern/discovery-routing`, `concern/evidence`, `concern/human-control`, `concern/ownership`, `concern/schema-types`, `concern/trust`, `domain/build-release`, `domain/configuration`, `domain/llm`, `domain/observability`, `domain/protocols`, `domain/security`, `domain/shell-terminal`, `domain/storage`, `domain/ui-interaction`, `domain/web-retrieval`, `lifecycle/implemented`, `mechanism/policy`, `mechanism/registry`
- Aliases: `off`, `list`, `metadata`, `get`, `SkillSummary`, `SkillRegistration`, `isModelInvocable`, `isUserInvocable`, `whenToUse`, `ctx.skills.list`, `ctx.skills.get`, `disable-model-invocation`, `user-invocable`, `invocation: SkillInvocationPolicy`
- Regex: `(?i)(list|metadata|SkillSummary|SkillRegistration|isModelInvocable|isUserInvocable|whenToUse|ctx\.skills\.list)`

```bash
rg -n --pcre2 "(?i)(list|metadata|SkillSummary|SkillRegistration|isModelInvocable|isUserInvocable|whenToUse|ctx\\.skills\\.list)" source-deepseek-harness
rg -l --fixed-strings "class/feature" 03-agent-context
```

## Connected notes

- **`source-link`** — [0580. TUI skill slash command](0580-tui-skill-slash-command.md): The source note links to this decision directly.
- **`source-link`** — [0143. Skill system --- progressive disclosure instructions for agents](0143-skill-system-progressive-disclosure-instructions-for-agents.md): The source note links to this decision directly.
- **`shares-code-with`** — [0192. Skill catalog hot refresh](0192-skill-catalog-hot-refresh.md): Shares source implementation: `packages/skill/skill/src/index.ts`, `packages/skill/tool-skill`.
- **`shares-code-with`** — [0277. User-explicit skill invocation at the pre-step gesture boundary](0277-user-explicit-skill-invocation-at-the-pre-step-gesture-boundary.md): Shares source implementation: `packages/skill/skill/src/index.ts`, `packages/skill/tool-skill`.
- **`shares-code-with`** — [0116. The skill registry is host-held and layered per scope](0116-the-skill-registry-is-host-held-and-layered-per-scope.md): Shares source implementation: `packages/skill/skill/src/index.ts`, `packages/skill/tool-skill/src/index.ts`.
- **`shares-code-with`** — [0252. Web transcript marks context source, recall, and steering](0252-web-transcript-marks-context-source-recall-and-steering.md): Shares source implementation: `packages/skill/tool-skill/src/index.ts`, `packages/skill/tool-skill/src/invariant.ts`.
- **`shares-code-with`** — [0256. Producer-declared context forms](0256-producer-declared-context-forms.md): Shares source implementation: `packages/skill/tool-skill/src/index.ts`, `packages/skill/tool-skill/src/invariant.ts`.
- **`shares-code-with`** — [0262. Bundled dsh badge skill](0262-bundled-dsh-badge-skill.md): Shares source implementation: `packages/skill/tool-skill`, `packages/skill/tool-skill/src/index.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0204-independent-model-and-user-skill-invocation-policy.md`.
