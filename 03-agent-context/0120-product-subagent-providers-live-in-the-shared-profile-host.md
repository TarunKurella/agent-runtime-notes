---
id: "dsh-note-0120"
title: "Product subagent providers live in the shared profile host"
status: "implemented"
class: "architecture"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/architecture/2026-08-10-product-subagent-providers-in-shared-host.md"
implementation_evidence: "medium"
target_anchor: "Rust provider-adapter boundary"
tags:
  - "class/architecture"
  - "concern/boundary"
  - "concern/cancellation-timeout"
  - "concern/compatibility"
  - "concern/discovery-routing"
  - "concern/evidence"
  - "concern/lifecycle"
  - "concern/ownership"
  - "domain/build-release"
  - "domain/configuration"
  - "domain/extensions"
  - "domain/llm"
  - "domain/multi-agent"
  - "domain/protocols"
  - "domain/session-state"
  - "domain/shell-terminal"
  - "domain/testing"
  - "lifecycle/implemented"
  - "mechanism/generation"
  - "mechanism/registry"
aliases:
  - "subagents"
  - "ctx.subagents"
  - "claude-code"
  - "dsh-tool-subagent"
  - "subagent_codex"
  - "subagent_claude_code"
  - "PATH"
  - "Product subagent providers live in the shared profile host"
  - "architecture"
  - "boundary"
  - "cancellation timeout"
  - "compatibility"
  - "discovery routing"
  - "evidence"
search_regex: "(?i)(subagents|ctx\\.subagents|claude\\-code|dsh\\-tool\\-subagent|subagent_codex|subagent_claude_code|PATH|Product[- ]subagent[- ]providers[- ]live[- ]in[- ]the[- ]shared[- ]profile[- ]host)"
---

# 0120. Product subagent providers live in the shared profile host — implementation context

## Open this when

The Codex and Claude Code provider contracts were first shipped as independently installable packages that a deployment loaded beside the common subagent tool. Agent Presets later became the ordinary owner of one agent's model-visible tools, but a preset cannot safely own these product providers: ctx.subagents is a process registry, provider names are unique, and host consumers resolve the same registry across sessions. Requiring a person to edit both a Profile and a Preset would also make a generic preset row incomplete by itself. The placement decision must preserve two independent facts.

## Source decision

Every shipped Profile loads the fixed codex and claude-code providers once through the base bundle's host plane. Loading either plugin only registers a dormant backend; the corresponding Codex or Claude process starts on the first actual delegation call. Agent Presets independently contribute ordinary dsh-tool-subagent rows for subagent_codex and subagent_claude_code, so a preset can expose neither tool, either one, or both without changing the provider registry. This decision supersedes only the opt-in composition placement recorded by the provider-contract note.

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/architecture/2026-08-10-product-subagent-providers-in-shared-host.md](../02-notes/implemented/architecture/2026-08-10-product-subagent-providers-in-shared-host.md)
- Pinned source: [.agents/notes/implemented/architecture/2026-08-10-product-subagent-providers-in-shared-host.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/architecture/2026-08-10-product-subagent-providers-in-shared-host.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`packages/subagent/tool-subagent/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/subagent/tool-subagent/src/index.ts) | package entry point | Core file in the package named by the note: `packages/subagent/tool-subagent`. | `named-package-member` |
| [`packages/subagent/tool-subagent/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/subagent/tool-subagent/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/subagent/tool-subagent`. | `named-package-member` |
| [`packages/subagent/tool-subagent`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/packages/subagent/tool-subagent) | package or module directory | The note names this package or capability. | `named-package` |
| [`packages/acp/acp/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/acp/acp/src/index.ts) | package entry point | Defines `subagents`, a construct named by the note. | `symbol-definition` |
| [`packages/subagent/tool-subagent/README.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/subagent/tool-subagent/README.md) | package contract and examples | Core file in the package named by the note: `packages/subagent/tool-subagent`. | `named-package-member` |
| [`packages/subagent/tool-subagent/package.json`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/subagent/tool-subagent/package.json) | composition and configuration | Core file in the package named by the note: `packages/subagent/tool-subagent`. | `named-package-member` |
| [`pnpm-lock.yaml`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/pnpm-lock.yaml) | composition and configuration | Contains the exact code literal `dsh-tool-subagent` named by the note. | `exact-code-occurrence` |
| [`docs/tool-catalog.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/tool-catalog.md) | package contract and examples | Contains the exact code literal `dsh-tool-subagent` named by the note. | `exact-code-occurrence` |
| [`examples/package.json`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/examples/package.json) | composition and configuration | Contains the exact code literal `dsh-tool-subagent` named by the note. | `exact-code-occurrence` |
| [`docs/config-catalog.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/config-catalog.md) | package contract and examples | Contains the exact code literal `dsh-tool-subagent` named by the note. | `exact-code-occurrence` |
| [`docs/tool-catalog.zh.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/tool-catalog.zh.md) | package contract and examples | Contains the exact code literal `dsh-tool-subagent` named by the note. | `exact-code-occurrence` |
| [`docs/config-catalog.zh.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/config-catalog.zh.md) | package contract and examples | Contains the exact code literal `dsh-tool-subagent` named by the note. | `exact-code-occurrence` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `subagents` | `const` | [`packages/acp/acp/src/index.ts:376`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/acp/acp/src/index.ts#L376) | `const subagents = ctx.get('subagents') as ContinuableDrain \| undefined` |

### Tests and executable evidence

- No test file was tied to this note with enough confidence. Read the note's verification section and search the owning package's `tests/` directory.
- Source verification intent: The base Loader test proves both provider names register exactly once and no product process starts during Profile boot. Real Agent Preset composition covers none, Codex-only, Claude-only, and both tool sets, including generation isolation after an authored preset changes. Keyless ACP snapshots pin the model-visible tool schemas for one and both products, while provider tests separately prove native executable resolution, failure, cancellation, and process-tree quiescence.

## How to read the implementation

1. Start with [`packages/subagent/tool-subagent/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/subagent/tool-subagent/src/index.ts) because it has the strongest evidence link to the note.
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

### Smallest safe implementation slice

1. Express the semantic contract as Rust types and one owning boundary.
2. Add the smallest adapter or service needed by the existing loop; do not add a new model-facing tool unless the four-tool surface cannot express the job.
3. Record durable evidence before deriving summaries or UI state.
4. Add one happy-path test, one interruption test, and one replay/restart test where state survives a turn.
5. Keep the patch reversible until the source note's strongest failure case is proven in the Rust harness.

## Search handles

- Tags: `class/architecture`, `concern/boundary`, `concern/cancellation-timeout`, `concern/compatibility`, `concern/discovery-routing`, `concern/evidence`, `concern/lifecycle`, `concern/ownership`, `domain/build-release`, `domain/configuration`, `domain/extensions`, `domain/llm`, `domain/multi-agent`, `domain/protocols`, `domain/session-state`, `domain/shell-terminal`, `domain/testing`, `lifecycle/implemented`, `mechanism/generation`, `mechanism/registry`
- Aliases: `subagents`, `ctx.subagents`, `claude-code`, `dsh-tool-subagent`, `subagent_codex`, `subagent_claude_code`, `PATH`, `Product subagent providers live in the shared profile host`, `architecture`, `boundary`, `cancellation timeout`, `compatibility`, `discovery routing`, `evidence`
- Regex: `(?i)(subagents|ctx\.subagents|claude\-code|dsh\-tool\-subagent|subagent_codex|subagent_claude_code|PATH|Product[- ]subagent[- ]providers[- ]live[- ]in[- ]the[- ]shared[- ]profile[- ]host)`

```bash
rg -n --pcre2 "(?i)(subagents|ctx\\.subagents|claude\\-code|dsh\\-tool\\-subagent|subagent_codex|subagent_claude_code|PATH|Product[- ]subagent[- ]providers[- ]live[- ]in[- ]the[- ]shared[- ]profile[- ]host)" source-deepseek-harness
rg -l --fixed-strings "class/architecture" 03-agent-context
```

## Connected notes

- **`source-link`** — [0093. A session's agent is composed from a preset cordis.yml](0093-a-session-s-agent-is-composed-from-a-preset-cordis-yml.md): The source note links to this decision directly.
- **`source-link`** — [0249. Claude Code and Codex subagent backends](0249-claude-code-and-codex-subagent-backends.md): The source note links to this decision directly.
- **`shares-code-with`** — [0664. Retire the standalone subagent mock package](0664-retire-the-standalone-subagent-mock-package.md): Shares source implementation: `packages/subagent/tool-subagent`, `packages/subagent/tool-subagent/README.md`.
- **`shares-code-with`** — [0662. Drop unconsumed skill provider events](0662-drop-unconsumed-skill-provider-events.md): Shares source implementation: `packages/subagent/tool-subagent`, `packages/subagent/tool-subagent/README.md`.
- **`shares-code-with`** — [0289. Continuable delegation is background-first](0289-continuable-delegation-is-background-first.md): Shares source implementation: `packages/subagent/tool-subagent`, `packages/subagent/tool-subagent/README.md`.
- **`shares-code-with`** — [0026. Subagent provider-lifecycle events --- `subagent/provider-added` / `subagent/provider-removed`](0026-subagent-provider-lifecycle-events-subagent-provider-added-subagent-prov.md): Shares source implementation: `packages/acp/acp/src/index.ts`, `packages/subagent/tool-subagent`.
- **`shares-code-with`** — [0148. Background subagent tasks](0148-background-subagent-tasks.md): Shares source implementation: `packages/subagent/tool-subagent`, `packages/subagent/tool-subagent/src/index.ts`.
- **`shares-code-with`** — [0153. Configure subagent persona, tool visibility, and depth](0153-configure-subagent-persona-tool-visibility-and-depth.md): Shares source implementation: `packages/subagent/tool-subagent`, `packages/subagent/tool-subagent/src/index.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0120-product-subagent-providers-live-in-the-shared-profile-host.md`.
