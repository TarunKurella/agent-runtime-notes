---
id: "dsh-note-0435"
title: "Concrete prose names actors and recorded facts"
status: "implemented"
class: "process"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/process/2026-08-09-concrete-prose-names-actors-and-recorded-facts.md"
implementation_evidence: "medium"
target_anchor: "repository tests and engineering policy"
tags:
  - "class/process"
  - "concern/boundary"
  - "concern/concurrency"
  - "concern/evidence"
  - "concern/lifecycle"
  - "concern/ownership"
  - "concern/schema-types"
  - "concern/trust"
  - "domain/build-release"
  - "domain/llm"
  - "domain/protocols"
  - "domain/security"
  - "domain/session-state"
  - "domain/storage"
  - "lifecycle/implemented"
aliases:
  - "contract"
  - "Boundary"
  - "boundary"
  - "Concrete prose names actors and recorded facts"
  - "process"
  - "concurrency"
  - "evidence"
  - "lifecycle"
  - "ownership"
  - "schema types"
  - "trust"
  - "build release"
  - "llm"
  - "protocols"
search_regex: "(?i)(contract|Boundary|Concrete[- ]prose[- ]names[- ]actors[- ]and[- ]recorded[- ]facts|concurrency|evidence|lifecycle|ownership|schema[- ]types)"
---

# 0435. Concrete prose names actors and recorded facts — implementation context

## Open this when

Repository prose used abstract category labels where readers needed different concrete facts. The same label could mean earlier event seqs cited by a replacement, the provider and model that produced a message, the caller that supplied context, the file that supplied a configuration row, or the CI job that built a binary. Readers had to inspect code before they could tell which fact the sentence promised. Replacing one broad label with another would preserve that ambiguity.

## Source decision

Maintained prose names the exact actor, action, source, event, field, file, or process needed by the local contract. It states what was recorded and who or what recorded it. Writers apply a spoken-language check and replace words they would not use while explaining the same point to a colleague. The rule applies to Markdown, READMEs, active Agent Notes, JSDoc and comments, prompts, diagnostics, and user-visible strings. An audit judges each sentence separately; it does not replace a term across the repository with one preferred synonym.

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/process/2026-08-09-concrete-prose-names-actors-and-recorded-facts.md](../02-notes/implemented/process/2026-08-09-concrete-prose-names-actors-and-recorded-facts.md)
- Pinned source: [.agents/notes/implemented/process/2026-08-09-concrete-prose-names-actors-and-recorded-facts.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/process/2026-08-09-concrete-prose-names-actors-and-recorded-facts.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`packages/host/apiproxy/src/api-proxy.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/src/api-proxy.ts) | runtime implementation | Defines `boundary`, a construct named by the note. | `symbol-definition` |
| [`packages/api/gateway/src/client/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/api/gateway/src/client/index.ts) | package entry point | Defines `contract`, a construct named by the note. | `symbol-definition` |
| [`packages/goal/tool-goal/src/authority.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/goal/tool-goal/src/authority.ts) | runtime implementation | Defines `boundary`, a construct named by the note. | `symbol-definition` |
| [`packages/typert/generator/src/analyzer.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/typert/generator/src/analyzer.ts) | runtime implementation | Defines `boundary`, a construct named by the note. | `symbol-definition` |
| [`packages/e2b/subprocess-e2b/src/output.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/e2b/subprocess-e2b/src/output.ts) | runtime implementation | Defines `boundary`, a construct named by the note. | `symbol-definition` |
| [`packages/session/session-title/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/session/session-title/src/index.ts) | package entry point | Defines `boundary`, a construct named by the note. | `symbol-definition` |
| [`packages/extensions/tool-cordis/src/inspect.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/extensions/tool-cordis/src/inspect.ts) | runtime implementation | Defines `contract`, a construct named by the note. | `symbol-definition` |
| [`packages/typert/generator/src/cordis-catalog.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/typert/generator/src/cordis-catalog.ts) | runtime implementation | Defines `contract`, a construct named by the note. | `symbol-definition` |
| [`packages/client/ui-conversation/src/client/skeleton/InputBar.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/src/client/skeleton/InputBar.tsx) | runtime implementation | Defines `Boundary`, a construct named by the note. | `symbol-definition` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `contract` | `const` | [`packages/api/gateway/src/client/index.ts:369`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/api/gateway/src/client/index.ts#L369) | `const contract = descriptor.cancellation === undefined` |
| `Boundary` | `type` | [`packages/client/ui-conversation/src/client/skeleton/InputBar.tsx:588`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/src/client/skeleton/InputBar.tsx#L588) | `type Boundary =` |
| `boundary` | `const` | [`packages/e2b/subprocess-e2b/src/output.ts:26`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/e2b/subprocess-e2b/src/output.ts#L26) | `const boundary = this.pending.indexOf('\n')` |
| `contract` | `const` | [`packages/extensions/tool-cordis/src/inspect.ts:241`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/extensions/tool-cordis/src/inspect.ts#L241) | `const contract = documented.find(entry => entry.signature === signature)` |
| `boundary` | `const` | [`packages/goal/tool-goal/src/authority.ts:33`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/goal/tool-goal/src/authority.ts#L33) | `const boundary = events[index]` |
| `boundary` | `const` | [`packages/host/apiproxy/src/api-proxy.ts:2386`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/src/api-proxy.ts#L2386) | `const boundary = anchoredBoundary` |
| `boundary` | `const` | [`packages/session/session-title/src/index.ts:508`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/session/session-title/src/index.ts#L508) | `const boundary = session.events.findLast(event => event.type === 'step/start' \|\| event.type === 'step/end')` |
| `boundary` | `const` | [`packages/typert/generator/src/analyzer.ts:1031`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/typert/generator/src/analyzer.ts#L1031) | `const boundary = this.remoteBoundary(` |
| `contract` | `const` | [`packages/typert/generator/src/cordis-catalog.ts:768`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/typert/generator/src/cordis-catalog.ts#L768) | `const contract = parseJsDoc(method.jsDoc)` |
| `contract` | `const` | [`packages/typert/generator/src/cordis-catalog.ts:788`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/typert/generator/src/cordis-catalog.ts#L788) | `const contract = parseJsDoc(event.jsDoc)` |

### Tests and executable evidence

- [`apps/web/tests/scaffold.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/scaffold.ts) — A test under the owning area exercises or imports `shape`.
- [`scripts/ci-workflow.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/ci-workflow.spec.ts) — A test under the owning area exercises or imports `shape`.
- [`apps/web/tests/todo-row.snapshot.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/todo-row.snapshot.ts) — A test under the owning area exercises or imports `shape`.
- [`apps/web/tests/search-card.snapshot.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/search-card.snapshot.ts) — A test under the owning area exercises or imports `shape`.
- [`apps/web/tests/sidebar-scrollbar.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/sidebar-scrollbar.e2e.ts) — A test under the owning area exercises or imports `shape`.
- [`apps/cli/tests/web-agent-presets.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/cli/tests/web-agent-presets.e2e.ts) — A test under the owning area exercises or imports `shape`.
- [`apps/web/tests/shipped-composition.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/shipped-composition.e2e.ts) — A test under the owning area exercises or imports `shape`.
- [`packages/typert/loader/tests/loader.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/typert/loader/tests/loader.spec.ts) — A test under the owning area exercises or imports `Shape`.

## How to read the implementation

1. Start with [`packages/host/apiproxy/src/api-proxy.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/src/api-proxy.ts) because it has the strongest evidence link to the note.
2. Read public types and entry points before implementation details. Write down the contract in plain language.
3. Follow the named constructs into their callers and tests. Confirm which component can actually enforce the rule.
4. Reconstruct one failure path: cancellation, crash, retry, stale state, partial write, or incompatible provider input—whichever the note is protecting against.
5. Only then map the contract into the current Rust repository. Target paths must come from that repository, never from guesswork in this wiki.

## Rust harness port

**Likely integration point:** repository tests and engineering policy.

Preserve the behavior and ownership rule. Do not mechanically translate Cordis, TypeScript declaration merging, Node streams, or package topology into Rust.

Relevant standing rules:

- **`rust/append-log`** — Keep durable facts append-only and derive views from them. Separate the synchronous fact boundary from asynchronous persistence.
- **`rust/capability-security`** — Make granted capabilities explicit. On the Windows no-admin path, probe support at runtime and fail closed rather than silently running unsandboxed.
- **`rust/four-tool-surface`** — Preserve the four model tools: read, write, patch, and exec. Reuse internal capabilities without automatically exposing new model-facing tools.
- **`rust/newtype-ids`** — Represent identity domains with Rust newtypes; do not pass bare strings across session, call, turn, step, and agent boundaries.
- **`rust/process-discipline`** — Treat this as repository or release policy unless it changes a runtime contract. Do not add runtime machinery for a process-only lesson.
- **`rust/provider-adapter`** — Keep provider wire quirks inside adapters; the loop and durable event vocabulary remain provider-neutral.
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

- Tags: `class/process`, `concern/boundary`, `concern/concurrency`, `concern/evidence`, `concern/lifecycle`, `concern/ownership`, `concern/schema-types`, `concern/trust`, `domain/build-release`, `domain/llm`, `domain/protocols`, `domain/security`, `domain/session-state`, `domain/storage`, `lifecycle/implemented`
- Aliases: `contract`, `Boundary`, `boundary`, `Concrete prose names actors and recorded facts`, `process`, `concurrency`, `evidence`, `lifecycle`, `ownership`, `schema types`, `trust`, `build release`, `llm`, `protocols`
- Regex: `(?i)(contract|Boundary|Concrete[- ]prose[- ]names[- ]actors[- ]and[- ]recorded[- ]facts|concurrency|evidence|lifecycle|ownership|schema[- ]types)`

```bash
rg -n --pcre2 "(?i)(contract|Boundary|Concrete[- ]prose[- ]names[- ]actors[- ]and[- ]recorded[- ]facts|concurrency|evidence|lifecycle|ownership|schema[- ]types)" source-deepseek-harness
rg -l --fixed-strings "class/process" 03-agent-context
```

## Connected notes

- **`source-link`** — [0386. Documentation structure, tiers, and budgets](0386-documentation-structure-tiers-and-budgets.md): The source note links to this decision directly.
- **`shares-code-with`** — [0433. Standardize Chinese contract terminology on 约定](0433-standardize-chinese-contract-terminology-on-contract.md): Shares source implementation: `apps/web/tests/scaffold.ts`, `packages/api/gateway/src/client/index.ts`.
- **`shares-code-with`** — [0366. Preset cards clamp their description instead of sizing the roster](0366-preset-cards-clamp-their-description-instead-of-sizing-the-roster.md): Shares source implementation: `packages/host/apiproxy/src/api-proxy.ts`, `packages/session/session-title/src/index.ts`.
- **`shares-code-with`** — [0324. Multi-select custom answer composition](0324-multi-select-custom-answer-composition.md): Shares source implementation: `packages/api/gateway/src/client/index.ts`, `packages/host/apiproxy/src/api-proxy.ts`.
- **`shares-code-with`** — [0594. Fixed `Tool / <name>` header for tool-call cards](0594-fixed-tool-name-header-for-tool-call-cards.md): Shares source implementation: `packages/host/apiproxy/src/api-proxy.ts`, `packages/session/session-title/src/index.ts`.
- **`shares-code-with`** — [0199. Workspace Registration Deletion](0199-workspace-registration-deletion.md): Shares source implementation: `packages/host/apiproxy/src/api-proxy.ts`, `packages/typert/generator/src/cordis-catalog.ts`.
- **`shares-code-with`** — [0500. Keyless browser e2e lane for the web GUI](0500-keyless-browser-e2e-lane-for-the-web-gui.md): Shares source implementation: `apps/web/tests/scaffold.ts`.
- **`shares-code-with`** — [0130. Multiplex concurrent ACP sessions over one connection](0130-multiplex-concurrent-acp-sessions-over-one-connection.md): Shares source implementation: `packages/host/apiproxy/src/api-proxy.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0435-concrete-prose-names-actors-and-recorded-facts.md`.
