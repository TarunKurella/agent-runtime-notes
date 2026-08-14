---
id: "dsh-note-0585"
title: "TUI file-reference autocomplete"
status: "archived"
class: "feature"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/archived/feature/2026-07-23-tui-file-reference-autocomplete.md"
implementation_evidence: "lead-only"
target_anchor: "read/write/patch filesystem capability"
tags:
  - "class/feature"
  - "concern/boundary"
  - "concern/cancellation-timeout"
  - "concern/compatibility"
  - "concern/discovery-routing"
  - "concern/evidence"
  - "concern/performance"
  - "domain/build-release"
  - "domain/configuration"
  - "domain/filesystem"
  - "domain/llm"
  - "domain/observability"
  - "domain/session-state"
  - "domain/shell-terminal"
  - "domain/storage"
  - "domain/ui-interaction"
  - "domain/web-retrieval"
  - "lifecycle/archived"
  - "mechanism/policy"
  - "mechanism/projection"
aliases:
  - "rg --files"
  - "TUI file-reference autocomplete"
  - "feature"
  - "boundary"
  - "cancellation timeout"
  - "compatibility"
  - "discovery routing"
  - "evidence"
  - "performance"
  - "build release"
  - "configuration"
  - "filesystem"
  - "llm"
  - "observability"
search_regex: "(?i)(rg[- ]\\-\\-files|TUI[- ]file\\-reference[- ]autocomplete|feature|boundary|cancellation[- ]timeout|compatibility|discovery[- ]routing|evidence)"
---

# 0585. TUI file-reference autocomplete — implementation context

## Open this when

The TUI offered structured @session references but no dependable way to discover workspace paths while composing a prompt. Requiring users to remember exact paths made file-oriented requests unnecessarily awkward, while eagerly attaching every selected file would spend context before the model knew whether its contents were relevant and would hide the normal read observation from the tool transcript.

## Source decision

The TUI owns a bounded, cancellable host-workspace path index rooted at the active session's working directory. Typing @ at a token boundary fuzzy-matches files and directories; queries containing / list the named directory directly, accepting a directory continues completion, and paths containing whitespace use the @"path with spaces" form. Configuration controls result count, index size, and excluded directory basenames. The default exclusions are .git and node_modules; traversal does not follow directory symlinks or interpret ignore files. Selecting a file changes only the editor text.

## Decision status

Historical design. Use it to understand an older pressure or failure; locate the replacing decision before implementing anything.

- Raw note: [archived/feature/2026-07-23-tui-file-reference-autocomplete.md](../02-notes/archived/feature/2026-07-23-tui-file-reference-autocomplete.md)
- Pinned source: [.agents/notes/archived/feature/2026-07-23-tui-file-reference-autocomplete.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/archived/feature/2026-07-23-tui-file-reference-autocomplete.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

No implementation path could be confirmed from the pinned tree. Use the search handles below; do not invent a source location.

### Code constructs named by the note

The note names behavior or policy but no uniquely resolvable code declaration. Search by its aliases and inspect the listed package entry points.

### Tests and executable evidence

- [`vitest.e2e.config.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vitest.e2e.config.ts) — A test under the owning area exercises or imports `and`.
- [`scripts/translation-brief.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/translation-brief.spec.ts) — A test under the owning area exercises or imports `and`.
- [`scripts/gen-client-catalog.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/gen-client-catalog.spec.ts) — A test under the owning area exercises or imports `and`.
- [`scripts/translation-pairing.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/translation-pairing.spec.ts) — A test under the owning area exercises or imports `and`.
- [`scripts/verify-cordis-config.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/verify-cordis-config.spec.ts) — A test under the owning area exercises or imports `and`.

## How to read the implementation

1. Start from the source note and run the regex below across the pinned repository.
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
- **`rust/process-discipline`** — Treat this as repository or release policy unless it changes a runtime contract. Do not add runtime machinery for a process-only lesson.
- **`rust/provider-adapter`** — Keep provider wire quirks inside adapters; the loop and durable event vocabulary remain provider-neutral.
- **`rust/quickjs-boundary`** — QuickJS holds orchestration variables; Rust/Tokio owns async execution and exposes only explicitly granted capabilities.
- **`rust/tokio-cancellation`** — Thread one cancellation token through async work, close owned resources before reuse, and test cancellation at every await boundary.
- **`rust/trait-seam`** — Use a small trait at the capability boundary; keep policy and model-facing schemas outside the backend implementation.
- **Status gate** — Historical design. Use it to understand an older pressure or failure; locate the replacing decision before implementing anything.

### Smallest safe implementation slice

1. Express the semantic contract as Rust types and one owning boundary.
2. Add the smallest adapter or service needed by the existing loop; do not add a new model-facing tool unless the four-tool surface cannot express the job.
3. Record durable evidence before deriving summaries or UI state.
4. Add one happy-path test, one interruption test, and one replay/restart test where state survives a turn.
5. Keep the patch reversible until the source note's strongest failure case is proven in the Rust harness.

## Search handles

- Tags: `class/feature`, `concern/boundary`, `concern/cancellation-timeout`, `concern/compatibility`, `concern/discovery-routing`, `concern/evidence`, `concern/performance`, `domain/build-release`, `domain/configuration`, `domain/filesystem`, `domain/llm`, `domain/observability`, `domain/session-state`, `domain/shell-terminal`, `domain/storage`, `domain/ui-interaction`, `domain/web-retrieval`, `lifecycle/archived`, `mechanism/policy`, `mechanism/projection`
- Aliases: `rg --files`, `TUI file-reference autocomplete`, `feature`, `boundary`, `cancellation timeout`, `compatibility`, `discovery routing`, `evidence`, `performance`, `build release`, `configuration`, `filesystem`, `llm`, `observability`
- Regex: `(?i)(rg[- ]\-\-files|TUI[- ]file\-reference[- ]autocomplete|feature|boundary|cancellation[- ]timeout|compatibility|discovery[- ]routing|evidence)`

```bash
rg -n --pcre2 "(?i)(rg[- ]\\-\\-files|TUI[- ]file\\-reference[- ]autocomplete|feature|boundary|cancellation[- ]timeout|compatibility|discovery[- ]routing|evidence)" source-deepseek-harness
rg -l --fixed-strings "class/feature" 03-agent-context
```

## Connected notes

- **`shares-code-with`** — [0337. Todo-first composer context order](0337-todo-first-composer-context-order.md): Shares source implementation: `scripts/gen-client-catalog.spec.ts`, `scripts/translation-brief.spec.ts`.
- **`shares-code-with`** — [0431. Dual Wine and native Windows pull-request CI](0431-dual-wine-and-native-windows-pull-request-ci.md): Shares source implementation: `vitest.e2e.config.ts`.
- **`shares-code-with`** — [0406. Briefed minimal translation updates](0406-briefed-minimal-translation-updates.md): Shares source implementation: `scripts/translation-brief.spec.ts`, `scripts/translation-pairing.spec.ts`.
- **`shares-code-with`** — [0407. CI failover runbook --- hosted pools → in-house pool](0407-ci-failover-runbook-hosted-pools-in-house-pool.md): Shares source implementation: `vitest.e2e.config.ts`.
- **`shares-code-with`** — [0644. Personal staging maintenance skills](0644-personal-staging-maintenance-skills.md): Shares source implementation: `scripts/translation-pairing.spec.ts`.
- **`shares-code-with`** — [0437. Oxlint-only fix workflow](0437-oxlint-only-fix-workflow.md): Shares source implementation: `scripts/gen-client-catalog.spec.ts`, `scripts/translation-pairing.spec.ts`.
- **`same-design-pressure`** — [0564. Bash-backed grep and glob discovery tools](0564-bash-backed-grep-and-glob-discovery-tools.md): Shares design concerns: `concern/boundary`, `concern/cancellation-timeout`, `concern/compatibility`.
- **`same-design-pressure`** — [0500. Keyless browser e2e lane for the web GUI](0500-keyless-browser-e2e-lane-for-the-web-gui.md): Shares design concerns: `concern/boundary`, `concern/cancellation-timeout`, `concern/compatibility`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0585-tui-file-reference-autocomplete.md`.
