---
id: "dsh-note-0234"
title: "Third-party memory MCP examples"
status: "implemented"
class: "feature"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/feature/2026-07-31-third-party-memory-mcp-examples.md"
implementation_evidence: "medium"
target_anchor: "sandbox capability boundary"
tags:
  - "class/feature"
  - "concern/boundary"
  - "concern/cancellation-timeout"
  - "concern/compatibility"
  - "concern/discovery-routing"
  - "concern/evidence"
  - "concern/lifecycle"
  - "concern/ownership"
  - "concern/performance"
  - "concern/recovery"
  - "concern/schema-types"
  - "domain/build-release"
  - "domain/configuration"
  - "domain/extensions"
  - "domain/llm"
  - "domain/protocols"
  - "domain/security"
  - "domain/session-state"
  - "domain/storage"
  - "domain/testing"
  - "lifecycle/implemented"
  - "mechanism/policy"
  - "mechanism/registry"
aliases:
  - "env"
  - "examples/mcp-memory"
  - "@deepseek-ai/dsh-mcp-client"
  - "mcp__<serverName>__<rawName>"
  - "mcp__"
  - "DSH_*"
  - "config.env"
  - "1.3.0"
  - "2026.7.4"
  - "v1.20.0"
  - "~/.memorix/data"
  - "~/.engram"
  - "$HOME/.dsh-mcp-reference-memory.jsonl"
  - "ENGRAM_PROJECT"
search_regex: "(?i)(examples/mcp\\-memory|@deepseek\\-ai/dsh\\-mcp\\-client|mcp__<serverName>__<rawName>|mcp__|DSH_\\*|config\\.env|1\\.3\\.0|2026\\.7\\.4)"
---

# 0234. Third-party memory MCP examples — implementation context

## Open this when

A direct vendor integration made one provider's API, configuration, health behavior, and tool semantics part of DSH. That was too much product surface for a capability already expressible through MCP, and it would require repeating the same adaptation for every memory system. Users instead need a small, inspectable way to opt into one external memory server while preserving the generic MCP boundary. The acceptance bar is stronger than "the socket connects": each reference must support a real DSH write in session A, recall from the provider in a fresh DSH session B, and use of the recalled value.

## Source decision

Ship three default-off Cordis overlay examples under examples/mcp-memory: Memorix, MCP Reference Memory, and Engram. Every file inserts exactly one @deepseek-ai/dsh-mcp-client row. None is referenced by the shipped composition, and the CLI declares the generic bridge only so an explicitly selected overlay can resolve it. These third-party configurations are provided as interoperability examples only. Their inclusion does not imply endorsement, recommendation, partnership, or ongoing support by DeepSeek.

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/feature/2026-07-31-third-party-memory-mcp-examples.md](../02-notes/implemented/feature/2026-07-31-third-party-memory-mcp-examples.md)
- Pinned source: [.agents/notes/implemented/feature/2026-07-31-third-party-memory-mcp-examples.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/feature/2026-07-31-third-party-memory-mcp-examples.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`packages/mcp/mcp-client/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/mcp/mcp-client/src/index.ts) | package entry point | Core file in the package named by the note: `packages/mcp/mcp-client`. | `named-package-member` |
| [`packages/core/system-prompt/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/system-prompt/src/index.ts) | package entry point | Core file in the package named by the note: `packages/core/system-prompt`. | `named-package-member` |
| [`packages/mcp/mcp-client/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/mcp/mcp-client/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/mcp/mcp-client`. | `named-package-member` |
| [`packages/core/system-prompt/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/system-prompt/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/core/system-prompt`. | `named-package-member` |
| [`examples/mcp-memory/README.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/examples/mcp-memory/README.md) | package contract and examples | Entry point or contract under the directory named by the note: `examples/mcp-memory`. | `named-directory-member` |
| [`examples/mcp-memory`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/examples/mcp-memory) | package or module directory | The source note names this implementation area directly. | `named-directory` |
| [`packages/mcp/mcp-client`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/packages/mcp/mcp-client) | package or module directory | The note names this package or capability. | `named-package` |
| [`packages/core/system-prompt`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/packages/core/system-prompt) | package or module directory | The note names this package or capability. | `named-package` |
| [`packages/subprocess/subprocess/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/subprocess/subprocess/src/index.ts) | package entry point | Defines `env`, a construct named by the note. | `symbol-definition` |
| [`packages/mcp/mcp-client/README.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/mcp/mcp-client/README.md) | package contract and examples | Core file in the package named by the note: `packages/mcp/mcp-client`. | `named-package-member` |
| [`packages/mcp/mcp-client/package.json`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/mcp/mcp-client/package.json) | composition and configuration | Core file in the package named by the note: `packages/mcp/mcp-client`. | `named-package-member` |
| [`packages/core/system-prompt/README.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/system-prompt/README.md) | package contract and examples | Core file in the package named by the note: `packages/core/system-prompt`. | `named-package-member` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `env` | `const` | [`packages/subprocess/subprocess/src/index.ts:61`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/subprocess/subprocess/src/index.ts#L61) | `const env: Record<string, string> = {}` |

### Tests and executable evidence

- [`apps/cli/tests/memory-mcp-configs.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/cli/tests/memory-mcp-configs.spec.ts) — Contains the exact code literal `examples/mcp-memory` named by the note.

## How to read the implementation

1. Start with [`packages/mcp/mcp-client/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/mcp/mcp-client/src/index.ts) because it has the strongest evidence link to the note.
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

- Tags: `class/feature`, `concern/boundary`, `concern/cancellation-timeout`, `concern/compatibility`, `concern/discovery-routing`, `concern/evidence`, `concern/lifecycle`, `concern/ownership`, `concern/performance`, `concern/recovery`, `concern/schema-types`, `domain/build-release`, `domain/configuration`, `domain/extensions`, `domain/llm`, `domain/protocols`, `domain/security`, `domain/session-state`, `domain/storage`, `domain/testing`, `lifecycle/implemented`, `mechanism/policy`, `mechanism/registry`
- Aliases: `env`, `examples/mcp-memory`, `@deepseek-ai/dsh-mcp-client`, `mcp__<serverName>__<rawName>`, `mcp__`, `DSH_*`, `config.env`, `1.3.0`, `2026.7.4`, `v1.20.0`, `~/.memorix/data`, `~/.engram`, `$HOME/.dsh-mcp-reference-memory.jsonl`, `ENGRAM_PROJECT`
- Regex: `(?i)(examples/mcp\-memory|@deepseek\-ai/dsh\-mcp\-client|mcp__<serverName>__<rawName>|mcp__|DSH_\*|config\.env|1\.3\.0|2026\.7\.4)`

```bash
rg -n --pcre2 "(?i)(examples/mcp\\-memory|@deepseek\\-ai/dsh\\-mcp\\-client|mcp__<serverName>__<rawName>|mcp__|DSH_\\*|config\\.env|1\\.3\\.0|2026\\.7\\.4)" source-deepseek-harness
rg -l --fixed-strings "class/feature" 03-agent-context
```

## Connected notes

- **`shares-code-with`** — [0483. Remove the dedicated repository Plugin path](0483-remove-the-dedicated-repository-plugin-path.md): Shares source implementation: `packages/mcp/mcp-client`, `packages/mcp/mcp-client/README.md`.
- **`shares-code-with`** — [0210. Persistent Bash and string-replacement editor tools](0210-persistent-bash-and-string-replacement-editor-tools.md): Shares source implementation: `packages/core/system-prompt`, `packages/core/system-prompt/src/index.ts`.
- **`shares-code-with`** — [0212. Current sandbox policy context](0212-current-sandbox-policy-context.md): Shares source implementation: `packages/core/system-prompt`, `packages/core/system-prompt/src/index.ts`.
- **`shares-code-with`** — [0147. MCP client plugin --- connect to external MCP servers and bridge their tools](0147-mcp-client-plugin-connect-to-external-mcp-servers-and-bridge-their-tools.md): Shares source implementation: `packages/mcp/mcp-client/src/index.ts`, `packages/mcp/mcp-client/src/invariant.ts`.
- **`shares-code-with`** — [0036. Shared scoped-layer storage](0036-shared-scoped-layer-storage.md): Shares source implementation: `packages/core/system-prompt/src/index.ts`, `packages/core/system-prompt/src/invariant.ts`.
- **`shares-code-with`** — [0313. Web agents receive explicit runtime context](0313-web-agents-receive-explicit-runtime-context.md): Shares source implementation: `packages/core/system-prompt`, `packages/core/system-prompt/src/index.ts`.
- **`shares-code-with`** — [0252. Web transcript marks context source, recall, and steering](0252-web-transcript-marks-context-source-recall-and-steering.md): Shares source implementation: `packages/core/system-prompt/src/index.ts`, `packages/core/system-prompt/src/invariant.ts`.
- **`shares-code-with`** — [0249. Claude Code and Codex subagent backends](0249-claude-code-and-codex-subagent-backends.md): Shares source implementation: `packages/subprocess/subprocess/src/index.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0234-third-party-memory-mcp-examples.md`.
