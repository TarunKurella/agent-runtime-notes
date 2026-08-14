---
id: "dsh-note-0539"
title: "Prune unused skill registry API"
status: "rejected"
class: "simplification"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/rejected/simplification/2026-07-12-prune-unused-skill-registry-api.md"
implementation_evidence: "medium"
target_anchor: "read/write/patch filesystem capability"
tags:
  - "class/simplification"
  - "concern/boundary"
  - "concern/discovery-routing"
  - "concern/evidence"
  - "concern/ownership"
  - "concern/performance"
  - "concern/schema-types"
  - "concern/simplification"
  - "domain/build-release"
  - "domain/configuration"
  - "domain/extensions"
  - "domain/filesystem"
  - "domain/llm"
  - "domain/testing"
  - "lifecycle/rejected"
  - "mechanism/generation"
  - "mechanism/policy"
  - "mechanism/registry"
aliases:
  - "list"
  - "source"
  - "setup"
  - "metadata"
  - "provider"
  - "get"
  - "locator"
  - "disableModelInvocation"
  - "SkillRegistration"
  - "whenToUse"
  - "runtime"
  - "path"
  - "ctx.skills.register"
  - "SkillSummary.whenToUse"
search_regex: "(?i)(list|source|setup|metadata|provider|locator|disableModelInvocation|SkillRegistration)"
---

# 0539. Prune unused skill registry API — implementation context

## Open this when

The skill service's embedded-runtime subsystem has zero production caller of ctx.skills.register(). It adds a reserved runtime provider name, a runtime map/rank/source, duplicate policy, a second revision in cache keys, normalization, disposers, and tests alongside the provider contract every shipped skill already uses. SkillSummary.whenToUse and candidate/definition path are parsed and copied but never read by a production consumer: the model catalog renders name/description, resource loading uses resourceBase, and providers own their locator. The deliberately open metadata extension point stays.

## Source decision

Remove SkillRegistry.register(), SkillRegistration, the runtime pseudo-provider and reserved-name rules, runtime revisions/cache branches, and runtime-only source/rank normalization. Tests that need an embedded skill register a small real provider. Retain providerRevision as the in-flight discovery epoch, but key completed catalogs by cwd alone: every provider mutation synchronously clears the cache, and the post-await revision comparison already prevents inserting stale work.

## Decision status

Rejected design. Do not implement it as written. Reuse its failure analysis, constraints, and reversal condition.

- Raw note: [rejected/simplification/2026-07-12-prune-unused-skill-registry-api.md](../02-notes/rejected/simplification/2026-07-12-prune-unused-skill-registry-api.md)
- Pinned source: [.agents/notes/rejected/simplification/2026-07-12-prune-unused-skill-registry-api.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/rejected/simplification/2026-07-12-prune-unused-skill-registry-api.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`packages/client/runtime/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/runtime/src/index.ts) | package entry point | Core file in the package named by the note: `packages/client/runtime`. | `named-package-member` |
| [`packages/client/runtime/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/runtime/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/client/runtime`. | `named-package-member` |
| [`packages/client/runtime/src/client/sessions/lineage.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/runtime/src/client/sessions/lineage.ts) | runtime implementation | Core file in the package named by the note: `packages/client/runtime`. Defines `list`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/client/runtime/src/client/sessions/provide.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/runtime/src/client/sessions/provide.ts) | runtime implementation | Core file in the package named by the note: `packages/client/runtime`. Defines `source`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/client/runtime/src/client/sessions/manager.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/runtime/src/client/sessions/manager.ts) | runtime implementation | Core file in the package named by the note: `packages/client/runtime`. Defines `source`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/client/runtime/src/client/sessions/context-provenance.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/runtime/src/client/sessions/context-provenance.ts) | runtime implementation | Core file in the package named by the note: `packages/client/runtime`. Defines `list`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/client/runtime`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/packages/client/runtime) | package or module directory | The note names this package or capability. | `named-package` |
| [`vendor/hmr/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/hmr/src/index.ts) | package entry point | Defines `path`, a construct named by the note. | `symbol-definition` |
| [`vendor/cordis/src/registry.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/cordis/src/registry.ts) | runtime implementation | Defines `runtime`, a construct named by the note. | `symbol-definition` |
| [`packages/llm/llm/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/src/index.ts) | package entry point | Defines `provider`, a construct named by the note. | `symbol-definition` |
| [`packages/skill/skill/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/skill/skill/src/index.ts) | package entry point | Defines `whenToUse`, a construct named by the note. Defines `SkillRegistration`, a construct named by the note. | `symbol-definition` |
| [`packages/host/apiproxy/src/api-proxy.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/src/api-proxy.ts) | runtime implementation | Defines `metadata`, a construct named by the note. | `symbol-definition` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `list` | `const` | [`packages/client/runtime/src/client/sessions/context-provenance.ts:45`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/runtime/src/client/sessions/context-provenance.ts#L45) | `const list = source[member]` |
| `list` | `const` | [`packages/client/runtime/src/client/sessions/lineage.ts:61`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/runtime/src/client/sessions/lineage.ts#L61) | `const list = children.get(s.parentSessionId) ?? []` |
| `source` | `const` | [`packages/client/runtime/src/client/sessions/manager.ts:584`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/runtime/src/client/sessions/manager.ts#L584) | `const source = this.summaries.find(s => s.sessionId === opts.sessionId)` |
| `source` | `const` | [`packages/client/runtime/src/client/sessions/provide.ts:146`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/runtime/src/client/sessions/provide.ts#L146) | `const source = contributedHooks[name]` |
| `setup` | `const` | [`packages/e2b/subprocess-e2b/src/index.ts:174`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/e2b/subprocess-e2b/src/index.ts#L174) | `const setup: TerminalSetup = { done: done.promise, controller: new AbortController() }` |
| `metadata` | `const` | [`packages/host/apiproxy/src/api-proxy.ts:555`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/src/api-proxy.ts#L555) | `const metadata = sessionListMetadata(session.events)` |
| `provider` | `const` | [`packages/llm/llm/src/index.ts:632`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/src/index.ts#L632) | `const provider = registration.provider.id` |
| `get` | `const` | [`packages/shell/tool-bash-persistent/src/index.ts:227`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/shell/tool-bash-persistent/src/index.ts#L227) | `const get = (owner: Agent, signal: AbortSignal): Promise<TerminalSessionId> => {` |
| `locator` | `const` | [`packages/skill/skill-filesystem/src/index.ts:207`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/skill/skill-filesystem/src/index.ts#L207) | `const locator = candidate.locator as LocalLocator` |
| `disableModelInvocation` | `const` | [`packages/skill/skill-filesystem/src/index.ts:996`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/skill/skill-filesystem/src/index.ts#L996) | `const disableModelInvocation = frontmatterBoolean(data, 'disable-model-invocation')` |
| `SkillRegistration` | `type` | [`packages/skill/skill/src/index.ts:96`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/skill/skill/src/index.ts#L96) | `export type SkillRegistration = Omit<SkillDefinition, 'invocation' \| 'provider'> & {` |
| `whenToUse` | `const` | [`packages/skill/skill/src/index.ts:752`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/skill/skill/src/index.ts#L752) | `const whenToUse = skill.whenToUse` |
| `runtime` | `const` | [`vendor/cordis/src/registry.ts:260`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/cordis/src/registry.ts#L260) | `const runtime = key && this._internal.get(key)` |
| `path` | `const` | [`vendor/hmr/src/index.ts:514`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/hmr/src/index.ts#L514) | `const path = relative(this.baseDir, fileURLToPath(filename))` |

### Tests and executable evidence

- [`packages/skill/skill/tests/skill.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/skill/skill/tests/skill.spec.ts) — A test under the owning area exercises or imports `whenToUse`.
- [`packages/skill/skill-filesystem/tests/skill-filesystem.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/skill/skill-filesystem/tests/skill-filesystem.spec.ts) — A test under the owning area exercises or imports `disableModelInvocation`.
- Source verification intent: Skill collection has one provider-backed path, a cwd-only completed-cache key, and a revision epoch only for in-flight invalidation; retained skill fields have a production reader or a recorded deliberate extension contract. Agent-scoped prompt sections, variables, tool providers, tool guards, and structured-output commit behavior in native and Code Mode remain unchanged. Typecheck, coverage, snapshots, doc-sync, module-graph verification, build, and hygiene pass.

## How to read the implementation

1. Start with [`packages/client/runtime/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/runtime/src/index.ts) because it has the strongest evidence link to the note.
2. Read public types and entry points before implementation details. Write down the contract in plain language.
3. Follow the named constructs into their callers and tests. Confirm which component can actually enforce the rule.
4. Reconstruct one failure path: cancellation, crash, retry, stale state, partial write, or incompatible provider input—whichever the note is protecting against.
5. Only then map the contract into the current Rust repository. Target paths must come from that repository, never from guesswork in this wiki.

## Rust harness port

**Likely integration point:** read/write/patch filesystem capability.

Preserve the behavior and ownership rule. Do not mechanically translate Cordis, TypeScript declaration merging, Node streams, or package topology into Rust.

Relevant standing rules:

- **`rust/capability-security`** — Make granted capabilities explicit. On the Windows no-admin path, probe support at runtime and fail closed rather than silently running unsandboxed.
- **`rust/four-tool-surface`** — Preserve the four model tools: read, write, patch, and exec. Reuse internal capabilities without automatically exposing new model-facing tools.
- **`rust/newtype-ids`** — Represent identity domains with Rust newtypes; do not pass bare strings across session, call, turn, step, and agent boundaries.
- **`rust/process-discipline`** — Treat this as repository or release policy unless it changes a runtime contract. Do not add runtime machinery for a process-only lesson.
- **`rust/provider-adapter`** — Keep provider wire quirks inside adapters; the loop and durable event vocabulary remain provider-neutral.
- **`rust/quickjs-boundary`** — QuickJS holds orchestration variables; Rust/Tokio owns async execution and exposes only explicitly granted capabilities.
- **`rust/serde-validation`** — Parse untrusted input into strict serde types and validate semantic constraints before executing side effects.
- **`rust/tagged-enums`** — Use serde-tagged enums for a closed wire vocabulary. Add an explicit extension envelope only where runtime extension is truly required.
- **`rust/trait-seam`** — Use a small trait at the capability boundary; keep policy and model-facing schemas outside the backend implementation.
- **Status gate** — Rejected design. Do not implement it as written. Reuse its failure analysis, constraints, and reversal condition.

### Smallest safe implementation slice

1. Express the semantic contract as Rust types and one owning boundary.
2. Add the smallest adapter or service needed by the existing loop; do not add a new model-facing tool unless the four-tool surface cannot express the job.
3. Record durable evidence before deriving summaries or UI state.
4. Add one happy-path test, one interruption test, and one replay/restart test where state survives a turn.
5. Keep the patch reversible until the source note's strongest failure case is proven in the Rust harness.

## Search handles

- Tags: `class/simplification`, `concern/boundary`, `concern/discovery-routing`, `concern/evidence`, `concern/ownership`, `concern/performance`, `concern/schema-types`, `concern/simplification`, `domain/build-release`, `domain/configuration`, `domain/extensions`, `domain/filesystem`, `domain/llm`, `domain/testing`, `lifecycle/rejected`, `mechanism/generation`, `mechanism/policy`, `mechanism/registry`
- Aliases: `list`, `source`, `setup`, `metadata`, `provider`, `get`, `locator`, `disableModelInvocation`, `SkillRegistration`, `whenToUse`, `runtime`, `path`, `ctx.skills.register`, `SkillSummary.whenToUse`
- Regex: `(?i)(list|source|setup|metadata|provider|locator|disableModelInvocation|SkillRegistration)`

```bash
rg -n --pcre2 "(?i)(list|source|setup|metadata|provider|locator|disableModelInvocation|SkillRegistration)" source-deepseek-harness
rg -l --fixed-strings "class/simplification" 03-agent-context
```

## Connected notes

- **`source-link`** — [0031. The agent is a registration scope](0031-the-agent-is-a-registration-scope.md): The source note links to this decision directly.
- **`shares-code-with`** — [0010. Filesystem capability seam --- ctx.fs, local backend, and model-facing filesystem tools](0010-filesystem-capability-seam-ctx-fs-local-backend-and-model-facing-filesys.md): Shares source implementation: `packages/llm/llm/src/index.ts`, `vendor/cordis/src/registry.ts`.
- **`shares-code-with`** — [0388. Export JSDoc gate](0388-export-jsdoc-gate.md): Shares source implementation: `packages/llm/llm/src/index.ts`, `vendor/cordis/src/registry.ts`.
- **`shares-code-with`** — [0143. Skill system --- progressive disclosure instructions for agents](0143-skill-system-progressive-disclosure-instructions-for-agents.md): Shares source implementation: `packages/client/runtime/src/index.ts`, `packages/client/runtime/src/invariant.ts`.
- **`shares-code-with`** — [0398. GUI testing system --- the three-tier structure](0398-gui-testing-system-the-three-tier-structure.md): Shares source implementation: `packages/client/runtime/src/index.ts`, `packages/client/runtime/src/invariant.ts`.
- **`shares-code-with`** — [0131. Code Mode --- the model writes TypeScript against the tool registry](0131-code-mode-the-model-writes-typescript-against-the-tool-registry.md): Shares source implementation: `packages/llm/llm/src/index.ts`.
- **`shares-code-with`** — [0516. Migrate simple unary API Proxy calls to business Remote services](0516-migrate-simple-unary-api-proxy-calls-to-business-remote-services.md): Shares source implementation: `packages/llm/llm/src/index.ts`.
- **`shares-code-with`** — [0487. parseCmdline runs the program's own commander action](0487-parsecmdline-runs-the-program-s-own-commander-action.md): Shares source implementation: `vendor/cordis/src/registry.ts`, `vendor/hmr/src/index.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0539-prune-unused-skill-registry-api.md`.
