---
id: "dsh-note-0453"
title: "Tighten the hook-protocol contract --- dialect, discarded fields, double defaults, and lib-owned `hook/result` semantics"
status: "implemented"
class: "simplification"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/simplification/2026-07-04-tighten-hook-protocol-contract.md"
implementation_evidence: "high"
target_anchor: "append-only event and session store"
tags:
  - "class/simplification"
  - "concern/boundary"
  - "concern/cancellation-timeout"
  - "concern/evidence"
  - "concern/lifecycle"
  - "concern/ownership"
  - "concern/recovery"
  - "concern/schema-types"
  - "domain/build-release"
  - "domain/configuration"
  - "domain/extensions"
  - "domain/multi-agent"
  - "domain/protocols"
  - "domain/session-state"
  - "domain/storage"
  - "domain/testing"
  - "lifecycle/implemented"
  - "mechanism/event-sourcing"
  - "mechanism/projection"
aliases:
  - "summarize"
  - "durationMs"
  - "decision"
  - "BLOCKING_EXIT_CODE"
  - "stopReason"
  - "HookResultRecord"
  - "DEFAULT_STDERR_SUMMARY_MAX_CHARS"
  - "appendHookResult"
  - "stderrSummary"
  - "dialect"
  - "additionalContext"
  - "DEFAULT_HOOK_TIMEOUT_MS"
  - "HookDialect"
  - "HookOutput"
search_regex: "(?i)(summarize|durationMs|decision|BLOCKING_EXIT_CODE|stopReason|HookResultRecord|DEFAULT_STDERR_SUMMARY_MAX_CHARS|appendHookResult)"
---

# 0453. Tighten the hook-protocol contract --- dialect, discarded fields, double defaults, and lib-owned `hook/result` semantics — implementation context

## Open this when

Four pieces of the dsh-hook-protocol/bridge contract missed the discipline the subagent-observe-enrich Agent Note records --- it dropped an agentType lifecycle field for lacking a consumer, and these failed the same test: HookDialect's 'native' variant (packages/hooks/hook-protocol/src/types.ts) had zero producers --- the bridges stamp 'claude' and 'codex'; the only 'native' constructor anywhere was the lib's own unit test.

## Source decision

HookDialect is the closed bridge set, 'claude' | 'codex'; HookOutput omits unsupported suppressOutput. hook/result.durationMs remains durable audit timing and is normalized only in snapshots. Reference defaults live once in DEFAULT_HOOK_TIMEOUT_MS and DEFAULT_STDERR_SUMMARY_MAX_CHARS. HookResultRecord and appendHookResult own stderr summarization and decision derivation for both bridges. BLOCKING_EXIT_CODE is codec-internal.

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/simplification/2026-07-04-tighten-hook-protocol-contract.md](../02-notes/implemented/simplification/2026-07-04-tighten-hook-protocol-contract.md)
- Pinned source: [.agents/notes/implemented/simplification/2026-07-04-tighten-hook-protocol-contract.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/simplification/2026-07-04-tighten-hook-protocol-contract.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`packages/hooks/hooks-codex/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/hooks/hooks-codex/src/index.ts) | package entry point | The source note names this file directly. Defines `defaultTimeoutMs`, a construct named by the note. | `named-file, symbol-definition` |
| [`packages/hooks/hook-protocol/src/types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/hooks/hook-protocol/src/types.ts) | public types and contract | The source note names this file directly. Core file in the package named by the note: `packages/hooks/hook-protocol`. | `named-file, named-package-member, symbol-definition` |
| [`packages/hooks/hooks-claude-code/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/hooks/hooks-claude-code/src/index.ts) | package entry point | The source note names this file directly. | `named-file` |
| [`packages/hooks/hook-protocol/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/hooks/hook-protocol/src/index.ts) | package entry point | Core file in the package named by the note: `packages/hooks/hook-protocol`. | `named-package-member` |
| [`packages/hooks/hook-protocol/src/merge.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/hooks/hook-protocol/src/merge.ts) | runtime implementation | Core file in the package named by the note: `packages/hooks/hook-protocol`. Defines `additionalContext`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/hooks/hook-protocol/src/codec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/hooks/hook-protocol/src/codec.ts) | runtime implementation | Core file in the package named by the note: `packages/hooks/hook-protocol`. Defines `stopReason`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/hooks/hook-protocol/src/events.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/hooks/hook-protocol/src/events.ts) | runtime implementation | Core file in the package named by the note: `packages/hooks/hook-protocol`. Defines `stderrSummary`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/hooks/hook-protocol/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/hooks/hook-protocol/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/hooks/hook-protocol`. Defines `dialect`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/hooks/hook-protocol/src/runner.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/hooks/hook-protocol/src/runner.ts) | runtime implementation | Defines `DEFAULT_HOOK_TIMEOUT_MS`, a construct named by the note. | `symbol-definition` |
| [`packages/hooks/hook-protocol`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/packages/hooks/hook-protocol) | package or module directory | The note names this package or capability. | `named-package` |
| [`packages/core/tools/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/index.ts) | package entry point | Defines `decision`, a construct named by the note. | `symbol-definition` |
| [`packages/bundle/headless/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/bundle/headless/src/index.ts) | package entry point | Defines `summarize`, a construct named by the note. | `symbol-definition` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `summarize` | `function` | [`packages/bundle/headless/src/index.ts:61`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/bundle/headless/src/index.ts#L61) | `function summarize(events: readonly SessionEvent[], firstSeq: number): RunOutcome {` |
| `durationMs` | `const` | [`packages/client/ui-trajectory/src/client/timeline.ts:58`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-trajectory/src/client/timeline.ts#L58) | `const durationMs = finite(cell.timeSeconds)` |
| `decision` | `const` | [`packages/core/tools/src/index.ts:1743`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/index.ts#L1743) | `const decision = await this.ctx.waterfall(` |
| `BLOCKING_EXIT_CODE` | `const` | [`packages/hooks/hook-protocol/src/codec.ts:11`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/hooks/hook-protocol/src/codec.ts#L11) | `const BLOCKING_EXIT_CODE = 2` |
| `stopReason` | `const` | [`packages/hooks/hook-protocol/src/codec.ts:100`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/hooks/hook-protocol/src/codec.ts#L100) | `const stopReason = str(parsed, 'stopReason')` |
| `HookResultRecord` | `interface` | [`packages/hooks/hook-protocol/src/events.ts:27`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/hooks/hook-protocol/src/events.ts#L27) | `export interface HookResultRecord {` |
| `DEFAULT_STDERR_SUMMARY_MAX_CHARS` | `const` | [`packages/hooks/hook-protocol/src/events.ts:53`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/hooks/hook-protocol/src/events.ts#L53) | `export const DEFAULT_STDERR_SUMMARY_MAX_CHARS = 500` |
| `appendHookResult` | `function` | [`packages/hooks/hook-protocol/src/events.ts:92`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/hooks/hook-protocol/src/events.ts#L92) | `export function appendHookResult(session: Session, record: HookResultRecord): void {` |
| `stderrSummary` | `const` | [`packages/hooks/hook-protocol/src/events.ts:94`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/hooks/hook-protocol/src/events.ts#L94) | `const stderrSummary = summarizeStderr(output.stderr, record.stderrSummaryMaxChars)` |
| `dialect` | `const` | [`packages/hooks/hook-protocol/src/invariant.ts:45`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/hooks/hook-protocol/src/invariant.ts#L45) | `const dialect: string = event.data.dialect` |
| `additionalContext` | `const` | [`packages/hooks/hook-protocol/src/merge.ts:68`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/hooks/hook-protocol/src/merge.ts#L68) | `const additionalContext: string[] = []` |
| `DEFAULT_HOOK_TIMEOUT_MS` | `const` | [`packages/hooks/hook-protocol/src/runner.ts:20`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/hooks/hook-protocol/src/runner.ts#L20) | `export const DEFAULT_HOOK_TIMEOUT_MS = 600_000` |
| `HookDialect` | `type` | [`packages/hooks/hook-protocol/src/types.ts:48`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/hooks/hook-protocol/src/types.ts#L48) | `export type HookDialect = 'claude-code' \| 'codex'` |
| `HookOutput` | `interface` | [`packages/hooks/hook-protocol/src/types.ts:89`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/hooks/hook-protocol/src/types.ts#L89) | `export interface HookOutput {` |
| `stderrSummaryMaxChars` | `const` | [`packages/hooks/hooks-codex/src/index.ts:83`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/hooks/hooks-codex/src/index.ts#L83) | `const stderrSummaryMaxChars = config.stderrSummaryMaxChars ?? DEFAULT_STDERR_SUMMARY_MAX_CHARS` |
| `defaultTimeoutMs` | `const` | [`packages/hooks/hooks-codex/src/index.ts:85`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/hooks/hooks-codex/src/index.ts#L85) | `const defaultTimeoutMs = config.defaultTimeoutMs ?? DEFAULT_HOOK_TIMEOUT_MS` |

### Tests and executable evidence

- [`packages/hooks/hook-protocol/tests/merge.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/hooks/hook-protocol/tests/merge.spec.ts) — A test under the owning area exercises or imports `dsh-hook-protocol`. A test under the owning area exercises or imports `systemMessage`.
- [`packages/hooks/hook-protocol/tests/codec.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/hooks/hook-protocol/tests/codec.spec.ts) — A test under the owning area exercises or imports `dsh-hook-protocol`. A test under the owning area exercises or imports `updatedInput`.
- [`packages/hooks/hook-protocol/tests/events.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/hooks/hook-protocol/tests/events.spec.ts) — A test under the owning area exercises or imports `dsh-hook-protocol`. A test under the owning area exercises or imports `codex`.
- [`packages/hooks/hook-protocol/tests/runner.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/hooks/hook-protocol/tests/runner.spec.ts) — A test under the owning area exercises or imports `dsh-hook-protocol`. A test under the owning area exercises or imports `defaultTimeoutMs`.
- [`packages/hooks/hook-protocol/tests/matcher.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/hooks/hook-protocol/tests/matcher.spec.ts) — A test under the owning area exercises or imports `dsh-hook-protocol`. A test under the owning area exercises or imports `claude`.
- [`packages/hooks/hooks-codex/tests/coverage-cases.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/hooks/hooks-codex/tests/coverage-cases.ts) — A test under the owning area exercises or imports `stderrSummaryMaxChars`.
- [`packages/hooks/hook-protocol/tests/detached.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/hooks/hook-protocol/tests/detached.spec.ts) — A test under the owning area exercises or imports `dsh-hook-protocol`.
- [`packages/hooks/hook-protocol/tests/invariant.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/hooks/hook-protocol/tests/invariant.spec.ts) — A test under the owning area exercises or imports `dsh-hook-protocol`. A test under the owning area exercises or imports `dialect`.
- Source verification intent: HookDialect contains only Claude and Codex, and suppressOutput is absent from source, parsed-field docs, and normalization. durationMs remains in events and fixtures with replay scrubbing. The 600_000 and 500 defaults each live once in the protocol library, per-hook timeout overrides still apply, and both bridge suites exercise the library-owned stderr truncation and decision rules.

## How to read the implementation

1. Start with [`packages/hooks/hooks-codex/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/hooks/hooks-codex/src/index.ts) because it has the strongest evidence link to the note.
2. Read public types and entry points before implementation details. Write down the contract in plain language.
3. Follow the named constructs into their callers and tests. Confirm which component can actually enforce the rule.
4. Reconstruct one failure path: cancellation, crash, retry, stale state, partial write, or incompatible provider input—whichever the note is protecting against.
5. Only then map the contract into the current Rust repository. Target paths must come from that repository, never from guesswork in this wiki.

## Rust harness port

**Likely integration point:** append-only event and session store.

Preserve the behavior and ownership rule. Do not mechanically translate Cordis, TypeScript declaration merging, Node streams, or package topology into Rust.

Relevant standing rules:

- **`rust/append-log`** — Keep durable facts append-only and derive views from them. Separate the synchronous fact boundary from asynchronous persistence.
- **`rust/four-tool-surface`** — Preserve the four model tools: read, write, patch, and exec. Reuse internal capabilities without automatically exposing new model-facing tools.
- **`rust/newtype-ids`** — Represent identity domains with Rust newtypes; do not pass bare strings across session, call, turn, step, and agent boundaries.
- **`rust/process-discipline`** — Treat this as repository or release policy unless it changes a runtime contract. Do not add runtime machinery for a process-only lesson.
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

- Tags: `class/simplification`, `concern/boundary`, `concern/cancellation-timeout`, `concern/evidence`, `concern/lifecycle`, `concern/ownership`, `concern/recovery`, `concern/schema-types`, `domain/build-release`, `domain/configuration`, `domain/extensions`, `domain/multi-agent`, `domain/protocols`, `domain/session-state`, `domain/storage`, `domain/testing`, `lifecycle/implemented`, `mechanism/event-sourcing`, `mechanism/projection`
- Aliases: `summarize`, `durationMs`, `decision`, `BLOCKING_EXIT_CODE`, `stopReason`, `HookResultRecord`, `DEFAULT_STDERR_SUMMARY_MAX_CHARS`, `appendHookResult`, `stderrSummary`, `dialect`, `additionalContext`, `DEFAULT_HOOK_TIMEOUT_MS`, `HookDialect`, `HookOutput`
- Regex: `(?i)(summarize|durationMs|decision|BLOCKING_EXIT_CODE|stopReason|HookResultRecord|DEFAULT_STDERR_SUMMARY_MAX_CHARS|appendHookResult)`

```bash
rg -n --pcre2 "(?i)(summarize|durationMs|decision|BLOCKING_EXIT_CODE|stopReason|HookResultRecord|DEFAULT_STDERR_SUMMARY_MAX_CHARS|appendHookResult)" source-deepseek-harness
rg -l --fixed-strings "class/simplification" 03-agent-context
```

## Connected notes

- **`source-link`** — [0560. Subagent lifecycle enrichment --- lastAssistantMessage (observe-only)](0560-subagent-lifecycle-enrichment-lastassistantmessage-observe-only.md): The source note links to this decision directly.
- **`source-link`** — [0140. Interception extension points --- the typed-Decision surface a hook programs against](0140-interception-extension-points-the-typed-decision-surface-a-hook-programs.md): The source note links to this decision directly.
- **`source-link`** — [0517. Pre-tool input rewrite --- a consistent design](0517-pre-tool-input-rewrite-a-consistent-design.md): The source note links to this decision directly.
- **`shares-code-with`** — [0139. dsh-hook-protocol --- the shared Claude Code / Codex hook wire-protocol core](0139-dsh-hook-protocol-the-shared-claude-code-codex-hook-wire-protocol-core.md): Shares source implementation: `packages/hooks/hook-protocol/src/events.ts`, `packages/hooks/hook-protocol/src/index.ts`.
- **`shares-code-with`** — [0679. Hook snapshot matrix --- end-to-end expected outputs for both bridges](0679-hook-snapshot-matrix-end-to-end-expected-outputs-for-both-bridges.md): Shares source implementation: `packages/hooks/hooks-codex/src/index.ts`.
- **`shares-code-with`** — [0303. Bind JSONL session identity before mutation](0303-bind-jsonl-session-identity-before-mutation.md): Shares source implementation: `packages/hooks/hook-protocol/src/merge.ts`.
- **`same-design-pressure`** — [0126. Repository naming contract and pre-release rename ledger](0126-repository-naming-contract-and-pre-release-rename-ledger.md): Shares design concerns: `concern/boundary`, `concern/cancellation-timeout`, `concern/evidence`.
- **`same-design-pressure`** — [0542. Dependency swaps rejected by the 2026-07 NIH audit](0542-dependency-swaps-rejected-by-the-2026-07-nih-audit.md): Shares design concerns: `concern/boundary`, `concern/cancellation-timeout`, `concern/evidence`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0453-tighten-the-hook-protocol-contract-dialect-discarded-fields-double-defau.md`.
