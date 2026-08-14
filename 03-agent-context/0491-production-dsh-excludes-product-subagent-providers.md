---
id: "dsh-note-0491"
title: "Production dsh excludes product subagent providers"
status: "implemented"
class: "simplification"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/simplification/2026-08-12-production-dsh-excludes-product-subagent-providers.md"
implementation_evidence: "medium"
target_anchor: "Rust provider-adapter boundary"
tags:
  - "class/simplification"
  - "concern/boundary"
  - "concern/discovery-routing"
  - "concern/evidence"
  - "concern/lifecycle"
  - "concern/schema-types"
  - "domain/extensions"
  - "domain/llm"
  - "domain/multi-agent"
  - "domain/testing"
  - "lifecycle/implemented"
aliases:
  - "@deepseek-ai/dsh"
  - "@deepseek-ai/dsh-base"
  - "Production dsh excludes product subagent providers"
  - "simplification"
  - "boundary"
  - "discovery routing"
  - "evidence"
  - "lifecycle"
  - "schema types"
  - "extensions"
  - "llm"
  - "multi agent"
  - "testing"
  - "implemented"
search_regex: "(?i)(@deepseek\\-ai/dsh|@deepseek\\-ai/dsh\\-base|Production[- ]dsh[- ]excludes[- ]product[- ]subagent[- ]providers|simplification|boundary|discovery[- ]routing|evidence|lifecycle)"
---

# 0491. Production dsh excludes product subagent providers — implementation context

## Open this when

@deepseek-ai/dsh receives the @deepseek-ai/dsh-base dependency closure. Including the Codex and Claude Code subagent providers there makes every production install download optional product integration code, including the Claude Agent SDK, even when neither integration is used.

## Source decision

This decision supersedes the shared-host placement: @deepseek-ai/dsh-base does not depend on or mount the Codex and Claude Code subagent providers. Their packages remain available for Profiles that install and mount them explicitly. Repository examples keep direct development dependencies so their explicit provider configurations continue to resolve.

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/simplification/2026-08-12-production-dsh-excludes-product-subagent-providers.md](../02-notes/implemented/simplification/2026-08-12-production-dsh-excludes-product-subagent-providers.md)
- Pinned source: [.agents/notes/implemented/simplification/2026-08-12-production-dsh-excludes-product-subagent-providers.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/simplification/2026-08-12-production-dsh-excludes-product-subagent-providers.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`packages/bundle/base/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/bundle/base/src/index.ts) | package entry point | Core file in the package named by the note: `packages/bundle/base`. | `named-package-member` |
| [`packages/bundle/base/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/bundle/base/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/bundle/base`. | `named-package-member` |
| [`apps/cli`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/apps/cli) | package or module directory | The note names this package or capability. | `named-package` |
| [`packages/bundle/base`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/packages/bundle/base) | package or module directory | The note names this package or capability. | `named-package` |
| [`apps/cli/README.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/cli/README.md) | package contract and examples | Core file in the package named by the note: `apps/cli`. | `named-package-member` |
| [`apps/cli/package.json`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/cli/package.json) | composition and configuration | Core file in the package named by the note: `apps/cli`. | `named-package-member` |
| [`packages/bundle/base/README.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/bundle/base/README.md) | package contract and examples | Core file in the package named by the note: `packages/bundle/base`. | `named-package-member` |
| [`packages/bundle/base/package.json`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/bundle/base/package.json) | composition and configuration | Core file in the package named by the note: `packages/bundle/base`. | `named-package-member` |

### Code constructs named by the note

The note names behavior or policy but no uniquely resolvable code declaration. Search by its aliases and inspect the listed package entry points.

### Tests and executable evidence

- No test file was tied to this note with enough confidence. Read the note's verification section and search the owning package's `tests/` directory.
- Source verification intent: The base bundle test rejects both provider dependencies and configuration rows. Cordis configuration validation requires explicit examples to declare the provider packages they name.

## How to read the implementation

1. Start with [`packages/bundle/base/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/bundle/base/src/index.ts) because it has the strongest evidence link to the note.
2. Read public types and entry points before implementation details. Write down the contract in plain language.
3. Follow the named constructs into their callers and tests. Confirm which component can actually enforce the rule.
4. Reconstruct one failure path: cancellation, crash, retry, stale state, partial write, or incompatible provider input—whichever the note is protecting against.
5. Only then map the contract into the current Rust repository. Target paths must come from that repository, never from guesswork in this wiki.

## Rust harness port

**Likely integration point:** Rust provider-adapter boundary.

Preserve the behavior and ownership rule. Do not mechanically translate Cordis, TypeScript declaration merging, Node streams, or package topology into Rust.

Relevant standing rules:

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

- Tags: `class/simplification`, `concern/boundary`, `concern/discovery-routing`, `concern/evidence`, `concern/lifecycle`, `concern/schema-types`, `domain/extensions`, `domain/llm`, `domain/multi-agent`, `domain/testing`, `lifecycle/implemented`
- Aliases: `@deepseek-ai/dsh`, `@deepseek-ai/dsh-base`, `Production dsh excludes product subagent providers`, `simplification`, `boundary`, `discovery routing`, `evidence`, `lifecycle`, `schema types`, `extensions`, `llm`, `multi agent`, `testing`, `implemented`
- Regex: `(?i)(@deepseek\-ai/dsh|@deepseek\-ai/dsh\-base|Production[- ]dsh[- ]excludes[- ]product[- ]subagent[- ]providers|simplification|boundary|discovery[- ]routing|evidence|lifecycle)`

```bash
rg -n --pcre2 "(?i)(@deepseek\\-ai/dsh|@deepseek\\-ai/dsh\\-base|Production[- ]dsh[- ]excludes[- ]product[- ]subagent[- ]providers|simplification|boundary|discovery[- ]routing|evidence|lifecycle)" source-deepseek-harness
rg -l --fixed-strings "class/simplification" 03-agent-context
```

## Connected notes

- **`source-link`** — [0120. Product subagent providers live in the shared profile host](0120-product-subagent-providers-live-in-the-shared-profile-host.md): The source note links to this decision directly.
- **`shares-code-with`** — [0313. Web agents receive explicit runtime context](0313-web-agents-receive-explicit-runtime-context.md): Shares source implementation: `packages/bundle/base`, `packages/bundle/base/README.md`.
- **`shares-code-with`** — [0597. `dsh meta` boots the TUI over the harness checkout](0597-dsh-meta-boots-the-tui-over-the-harness-checkout.md): Shares source implementation: `apps/cli`, `apps/cli/README.md`.
- **`shares-code-with`** — [0676. Explicit-config dsh entrypoint](0676-explicit-config-dsh-entrypoint.md): Shares source implementation: `apps/cli`, `apps/cli/README.md`.
- **`shares-code-with`** — [0579. Product-level TUI session resume](0579-product-level-tui-session-resume.md): Shares source implementation: `apps/cli`, `apps/cli/README.md`.
- **`shares-code-with`** — [0570. dsh tells the agent where its own source lives](0570-dsh-tells-the-agent-where-its-own-source-lives.md): Shares source implementation: `apps/cli`, `apps/cli/README.md`.
- **`shares-code-with`** — [0604. Versioned TUI first-run welcome](0604-versioned-tui-first-run-welcome.md): Shares source implementation: `apps/cli`, `apps/cli/README.md`.
- **`shares-code-with`** — [0115. headless is a direct core entry point](0115-headless-is-a-direct-core-entry-point.md): Shares source implementation: `packages/bundle/base`, `packages/bundle/base/src/index.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0491-production-dsh-excludes-product-subagent-providers.md`.
