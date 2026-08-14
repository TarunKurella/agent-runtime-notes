---
id: "dsh-note-0358"
title: "The minimal preset owns the complete RL agent composition"
status: "implemented"
class: "bug-fix"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/bug-fix/2026-08-10-minimal-preset-owns-rl-composition.md"
implementation_evidence: "high"
target_anchor: "sandbox capability boundary"
tags:
  - "class/bug-fix"
  - "concern/boundary"
  - "concern/cancellation-timeout"
  - "concern/discovery-routing"
  - "concern/evidence"
  - "concern/human-control"
  - "concern/ownership"
  - "concern/schema-types"
  - "concern/trust"
  - "domain/build-release"
  - "domain/configuration"
  - "domain/context"
  - "domain/extensions"
  - "domain/filesystem"
  - "domain/llm"
  - "domain/multi-agent"
  - "domain/protocols"
  - "domain/security"
  - "domain/session-state"
  - "domain/shell-terminal"
  - "domain/storage"
  - "domain/testing"
  - "domain/web-retrieval"
  - "lifecycle/implemented"
  - "mechanism/event-sourcing"
  - "mechanism/policy"
  - "mechanism/registry"
aliases:
  - "PromptSection"
  - "core-web.cordis.yml"
  - "str_replace_editor"
  - "fs-local"
  - "requireAbsolutePath"
  - "You are a helpful software engineer assistant."
  - "system-prompt/assemble"
  - "minimal.cordis.yml"
  - "The minimal preset owns the complete RL agent composition"
  - "bug fix"
  - "boundary"
  - "cancellation timeout"
  - "discovery routing"
  - "evidence"
search_regex: "(?i)(PromptSection|core\\-web\\.cordis\\.yml|str_replace_editor|fs\\-local|requireAbsolutePath|You[- ]are[- ]a[- ]helpful[- ]software[- ]engineer[- ]assistant\\.|system\\-prompt/assemble|minimal\\.cordis\\.yml)"
---

# 0358. The minimal preset owns the complete RL agent composition — implementation context

## Open this when

The shipped Web configuration offered two owners for the Claude SWE-compatible RL agent: a process-wide core-web.cordis.yml patch and the per-session minimal preset. Once agent presets became the agent-composition boundary, the preset's scoped deployment:persona shadowed the overlay's corrected global persona with stale coding-agent text. The overlay test mounted no preset, while the preset test booted without the overlay, so neither exercised the composition users selected. The split also hid other drift.

## Source decision

The shipped Web minimal preset is the sole Web owner of the RL agent composition. It declares an entry-local PTY registry and local backend, persistent bash with the RL environment description and 300-second timeout, and str_replace_editor. Tool presentation remains a deployment choice. The later bare two-tool runtime decision supersedes this note's original compaction and filesystem-provider choices: the current preset mounts an entry-local fs-local provider and no compaction backend. The editor accepts no requireAbsolutePath setting because absolute paths are its unconditional contract.

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/bug-fix/2026-08-10-minimal-preset-owns-rl-composition.md](../02-notes/implemented/bug-fix/2026-08-10-minimal-preset-owns-rl-composition.md)
- Pinned source: [.agents/notes/implemented/bug-fix/2026-08-10-minimal-preset-owns-rl-composition.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/bug-fix/2026-08-10-minimal-preset-owns-rl-composition.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`examples/jsonrpc-agent/minimal.cordis.yml`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/examples/jsonrpc-agent/minimal.cordis.yml) | composition and configuration | The source note names this file directly. | `named-file` |
| [`packages/fs/fs-local/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/fs-local/src/index.ts) | package entry point | Core file in the package named by the note: `packages/fs/fs-local`. | `named-package-member` |
| [`packages/fs/fs-local/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/fs-local/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/fs/fs-local`. | `named-package-member` |
| [`packages/fs/fs-local`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/packages/fs/fs-local) | package or module directory | The note names this package or capability. | `named-package` |
| [`packages/core/system-prompt/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/system-prompt/src/index.ts) | package entry point | Defines `PromptSection`, a construct named by the note. | `symbol-definition` |
| [`packages/fs/fs-local/README.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/fs-local/README.md) | package contract and examples | Core file in the package named by the note: `packages/fs/fs-local`. | `named-package-member` |
| [`packages/fs/fs-local/package.json`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/fs-local/package.json) | composition and configuration | Core file in the package named by the note: `packages/fs/fs-local`. | `named-package-member` |
| [`docs/agent-lifecycle.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/agent-lifecycle.md) | package contract and examples | Contains the exact code literal `system-prompt/assemble` named by the note. | `exact-code-occurrence` |
| [`scripts/gen-doc-graphs.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/gen-doc-graphs.ts) | repository automation | Contains the exact code literal `system-prompt/assemble` named by the note. | `exact-code-occurrence` |
| [`docs/agent-lifecycle.zh.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/agent-lifecycle.zh.md) | package contract and examples | Contains the exact code literal `system-prompt/assemble` named by the note. | `exact-code-occurrence` |
| [`docs/event-producer-consumer.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/event-producer-consumer.md) | package contract and examples | Contains the exact code literal `system-prompt/assemble` named by the note. | `exact-code-occurrence` |
| [`docs/subsystems/system-prompt.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/subsystems/system-prompt.md) | package contract and examples | Contains the exact code literal `system-prompt/assemble` named by the note. | `exact-code-occurrence` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `PromptSection` | `interface` | [`packages/core/system-prompt/src/index.ts:53`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/system-prompt/src/index.ts#L53) | `export interface PromptSection {` |

### Tests and executable evidence

- [`packages/fs/fs-local/tests/filesystem.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/fs-local/tests/filesystem.spec.ts) — A test under the owning area exercises or imports `fs-local`.
- Source verification intent: System-prompt and persona package tests prove final complete-section and runtime-context suppression, including waterfall mutation and duplicate rejection. The shipped-preset composition test asserts the exact prompt, Bash description, absolute editor schema, and two-tool catalog under the default native presentation. The keyless Web replay sends a real request through a minimal agent while global identity, Web-orientation text, dynamic policy contexts, and a test section are registered, asserts that no runtime-context snapshot exists, the entry-local filesystem is bare, and compaction is absent, then executes.

## How to read the implementation

1. Start with [`examples/jsonrpc-agent/minimal.cordis.yml`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/examples/jsonrpc-agent/minimal.cordis.yml) because it has the strongest evidence link to the note.
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
- **`rust/context-evidence`** — Compaction may shrink presentation, but it must not erase the evidence ledger, file activity, or durable session facts.
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

- Tags: `class/bug-fix`, `concern/boundary`, `concern/cancellation-timeout`, `concern/discovery-routing`, `concern/evidence`, `concern/human-control`, `concern/ownership`, `concern/schema-types`, `concern/trust`, `domain/build-release`, `domain/configuration`, `domain/context`, `domain/extensions`, `domain/filesystem`, `domain/llm`, `domain/multi-agent`, `domain/protocols`, `domain/security`, `domain/session-state`, `domain/shell-terminal`, `domain/storage`, `domain/testing`, `domain/web-retrieval`, `lifecycle/implemented`, `mechanism/event-sourcing`, `mechanism/policy`, `mechanism/registry`
- Aliases: `PromptSection`, `core-web.cordis.yml`, `str_replace_editor`, `fs-local`, `requireAbsolutePath`, `You are a helpful software engineer assistant.`, `system-prompt/assemble`, `minimal.cordis.yml`, `The minimal preset owns the complete RL agent composition`, `bug fix`, `boundary`, `cancellation timeout`, `discovery routing`, `evidence`
- Regex: `(?i)(PromptSection|core\-web\.cordis\.yml|str_replace_editor|fs\-local|requireAbsolutePath|You[- ]are[- ]a[- ]helpful[- ]software[- ]engineer[- ]assistant\.|system\-prompt/assemble|minimal\.cordis\.yml)`

```bash
rg -n --pcre2 "(?i)(PromptSection|core\\-web\\.cordis\\.yml|str_replace_editor|fs\\-local|requireAbsolutePath|You[- ]are[- ]a[- ]helpful[- ]software[- ]engineer[- ]assistant\\.|system\\-prompt/assemble|minimal\\.cordis\\.yml)" source-deepseek-harness
rg -l --fixed-strings "class/bug-fix" 03-agent-context
```

## Connected notes

- **`source-link`** — [0093. A session's agent is composed from a preset cordis.yml](0093-a-session-s-agent-is-composed-from-a-preset-cordis-yml.md): The source note links to this decision directly.
- **`source-link`** — [0210. Persistent Bash and string-replacement editor tools](0210-persistent-bash-and-string-replacement-editor-tools.md): The source note links to this decision directly.
- **`source-link`** — [0293. Minimal profiles use the bare two-tool runtime](0293-minimal-profiles-use-the-bare-two-tool-runtime.md): The source note links to this decision directly.
- **`shares-code-with`** — [0300. Preserve Windows DACLs during atomic file replacement](0300-preserve-windows-dacls-during-atomic-file-replacement.md): Shares source implementation: `packages/fs/fs-local`, `packages/fs/fs-local/README.md`.
- **`shares-code-with`** — [0550. Windows write-permission semantics --- inherited DACLs, not mode bits](0550-windows-write-permission-semantics-inherited-dacls-not-mode-bits.md): Shares source implementation: `packages/fs/fs-local`, `packages/fs/fs-local/README.md`.
- **`shares-code-with`** — [0321. Bound overwrite contextual-diff bases at the provider](0321-bound-overwrite-contextual-diff-bases-at-the-provider.md): Shares source implementation: `packages/fs/fs-local`, `packages/fs/fs-local/README.md`.
- **`shares-code-with`** — [0178. Web UI permission presets and approval answering](0178-web-ui-permission-presets-and-approval-answering.md): Shares source implementation: `packages/fs/fs-local/src/index.ts`, `packages/fs/fs-local/src/invariant.ts`.
- **`shares-code-with`** — [0201. Cross-workspace session resume](0201-cross-workspace-session-resume.md): Shares source implementation: `packages/fs/fs-local`, `packages/fs/fs-local/src/index.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0358-the-minimal-preset-owns-the-complete-rl-agent-composition.md`.
