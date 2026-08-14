---
id: "dsh-note-0395"
title: "Keep Agent Notes discoverable without a generated index"
status: "implemented"
class: "process"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/process/2026-07-19-remove-generated-agent-note-index.md"
implementation_evidence: "high"
target_anchor: "repository tests and engineering policy"
tags:
  - "class/process"
  - "concern/boundary"
  - "concern/compatibility"
  - "concern/discovery-routing"
  - "concern/evidence"
  - "concern/lifecycle"
  - "domain/build-release"
  - "domain/configuration"
  - "domain/filesystem"
  - "domain/ui-interaction"
  - "lifecycle/implemented"
  - "mechanism/generation"
  - "mechanism/policy"
  - "mechanism/projection"
aliases:
  - "scripts/agent-note-tree.ts"
  - "verify-agent-note-classification"
  - "INDEX.md"
  - "Keep Agent Notes discoverable without a generated index"
  - "process"
  - "boundary"
  - "compatibility"
  - "discovery routing"
  - "evidence"
  - "lifecycle"
  - "build release"
  - "configuration"
  - "filesystem"
  - "ui interaction"
search_regex: "(?i)(scripts/agent\\-note\\-tree\\.ts|verify\\-agent\\-note\\-classification|INDEX\\.md|Keep[- ]Agent[- ]Notes[- ]discoverable[- ]without[- ]a[- ]generated[- ]index|boundary|compatibility|discovery[- ]routing|evidence)"
---

# 0395. Keep Agent Notes discoverable without a generated index — implementation context

## Open this when

A committed Agent Note index duplicates facts already encoded by each file's lifecycle/class path, filename date, and H1. Every branch that adds, moves, or renames an otherwise unrelated Agent Note rewrites the same generated file, making that artifact a predictable merge hotspot. The centralized chronological list adds little discovery value beyond browsing the lifecycle/class tree or searching the repository, while its generator, renderer, command, and freshness check remain maintenance burden.

## Source decision

The lifecycle/class filesystem tree is the Agent Note inventory. README.md remains the curated entry point and contract, while ordinary tree navigation and repository search provide discovery. scripts/agent-note-tree.ts owns the closed lifecycle/class sets and structural walker. verify-agent-note-classification validates that tree and rejects the legacy homes and a root INDEX.md; it does not render or freshness-check a centralized list.

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/process/2026-07-19-remove-generated-agent-note-index.md](../02-notes/implemented/process/2026-07-19-remove-generated-agent-note-index.md)
- Pinned source: [.agents/notes/implemented/process/2026-07-19-remove-generated-agent-note-index.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/process/2026-07-19-remove-generated-agent-note-index.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`scripts/agent-note-tree.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/agent-note-tree.ts) | repository automation | The source note names this file directly. | `named-file` |

### Code constructs named by the note

The note names behavior or policy but no uniquely resolvable code declaration. Search by its aliases and inspect the listed package entry points.

### Tests and executable evidence

- No test file was tied to this note with enough confidence. Read the note's verification section and search the owning package's `tests/` directory.

## How to read the implementation

1. Start with [`scripts/agent-note-tree.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/agent-note-tree.ts) because it has the strongest evidence link to the note.
2. Read public types and entry points before implementation details. Write down the contract in plain language.
3. Follow the named constructs into their callers and tests. Confirm which component can actually enforce the rule.
4. Reconstruct one failure path: cancellation, crash, retry, stale state, partial write, or incompatible provider input—whichever the note is protecting against.
5. Only then map the contract into the current Rust repository. Target paths must come from that repository, never from guesswork in this wiki.

## Rust harness port

**Likely integration point:** repository tests and engineering policy.

Preserve the behavior and ownership rule. Do not mechanically translate Cordis, TypeScript declaration merging, Node streams, or package topology into Rust.

Relevant standing rules:

- **`rust/capability-security`** — Make granted capabilities explicit. On the Windows no-admin path, probe support at runtime and fail closed rather than silently running unsandboxed.
- **`rust/four-tool-surface`** — Preserve the four model tools: read, write, patch, and exec. Reuse internal capabilities without automatically exposing new model-facing tools.
- **`rust/process-discipline`** — Treat this as repository or release policy unless it changes a runtime contract. Do not add runtime machinery for a process-only lesson.
- **`rust/quickjs-boundary`** — QuickJS holds orchestration variables; Rust/Tokio owns async execution and exposes only explicitly granted capabilities.
- **`rust/trait-seam`** — Use a small trait at the capability boundary; keep policy and model-facing schemas outside the backend implementation.

### Smallest safe implementation slice

1. Express the semantic contract as Rust types and one owning boundary.
2. Add the smallest adapter or service needed by the existing loop; do not add a new model-facing tool unless the four-tool surface cannot express the job.
3. Record durable evidence before deriving summaries or UI state.
4. Add one happy-path test, one interruption test, and one replay/restart test where state survives a turn.
5. Keep the patch reversible until the source note's strongest failure case is proven in the Rust harness.

## Search handles

- Tags: `class/process`, `concern/boundary`, `concern/compatibility`, `concern/discovery-routing`, `concern/evidence`, `concern/lifecycle`, `domain/build-release`, `domain/configuration`, `domain/filesystem`, `domain/ui-interaction`, `lifecycle/implemented`, `mechanism/generation`, `mechanism/policy`, `mechanism/projection`
- Aliases: `scripts/agent-note-tree.ts`, `verify-agent-note-classification`, `INDEX.md`, `Keep Agent Notes discoverable without a generated index`, `process`, `boundary`, `compatibility`, `discovery routing`, `evidence`, `lifecycle`, `build release`, `configuration`, `filesystem`, `ui interaction`
- Regex: `(?i)(scripts/agent\-note\-tree\.ts|verify\-agent\-note\-classification|INDEX\.md|Keep[- ]Agent[- ]Notes[- ]discoverable[- ]without[- ]a[- ]generated[- ]index|boundary|compatibility|discovery[- ]routing|evidence)`

```bash
rg -n --pcre2 "(?i)(scripts/agent\\-note\\-tree\\.ts|verify\\-agent\\-note\\-classification|INDEX\\.md|Keep[- ]Agent[- ]Notes[- ]discoverable[- ]without[- ]a[- ]generated[- ]index|boundary|compatibility|discovery[- ]routing|evidence)" source-deepseek-harness
rg -l --fixed-strings "class/process" 03-agent-context
```

## Connected notes

- **`source-link`** — [0687. Agent Notes](0687-agent-notes.md): The source note links to this decision directly.
- **`shares-code-with`** — [0382. Classify Agent Notes by kind via path-encoded subdirectories](0382-classify-agent-notes-by-kind-via-path-encoded-subdirectories.md): Shares source implementation: `scripts/agent-note-tree.ts`.
- **`same-design-pressure`** — [0147. MCP client plugin --- connect to external MCP servers and bridge their tools](0147-mcp-client-plugin-connect-to-external-mcp-servers-and-bridge-their-tools.md): Shares design concerns: `concern/boundary`, `concern/compatibility`, `concern/discovery-routing`.
- **`same-design-pressure`** — [0092. Typert Gateway Targeted Method Calls](0092-typert-gateway-targeted-method-calls.md): Shares design concerns: `concern/boundary`, `concern/compatibility`, `concern/discovery-routing`.
- **`same-design-pressure`** — [0074. dsh source launch through the tsx ESM hook](0074-dsh-source-launch-through-the-tsx-esm-hook.md): Shares design concerns: `concern/boundary`, `concern/compatibility`, `concern/discovery-routing`.
- **`same-design-pressure`** — [0458. Plan-specific collaboration state](0458-plan-specific-collaboration-state.md): Shares design concerns: `concern/boundary`, `concern/compatibility`, `concern/discovery-routing`.
- **`same-design-pressure`** — [0496. Real-API e2e in CI against the external DeepSeek API](0496-real-api-e2e-in-ci-against-the-external-deepseek-api.md): Shares design concerns: `concern/boundary`, `concern/compatibility`, `concern/discovery-routing`.
- **`same-design-pressure`** — [0500. Keyless browser e2e lane for the web GUI](0500-keyless-browser-e2e-lane-for-the-web-gui.md): Shares design concerns: `concern/boundary`, `concern/compatibility`, `concern/discovery-routing`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0395-keep-agent-notes-discoverable-without-a-generated-index.md`.
