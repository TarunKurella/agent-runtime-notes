---
id: "dsh-note-0139"
title: "dsh-hook-protocol --- the shared Claude Code / Codex hook wire-protocol core"
status: "implemented"
class: "feature"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/feature/2026-06-30-hook-protocol-lib.md"
implementation_evidence: "medium"
target_anchor: "sandbox capability boundary"
tags:
  - "class/feature"
  - "concern/boundary"
  - "concern/cancellation-timeout"
  - "concern/lifecycle"
  - "concern/ownership"
  - "concern/schema-types"
  - "concern/trust"
  - "domain/build-release"
  - "domain/configuration"
  - "domain/context"
  - "domain/extensions"
  - "domain/llm"
  - "domain/observability"
  - "domain/protocols"
  - "domain/security"
  - "domain/session-state"
  - "domain/shell-terminal"
  - "domain/storage"
  - "lifecycle/implemented"
  - "mechanism/event-sourcing"
  - "mechanism/projection"
  - "mechanism/registry"
aliases:
  - "json"
  - "shell"
  - "PreStepDecision"
  - "SurfaceEventType"
  - "PreToolDecision"
  - "PostToolDecision"
  - "decision"
  - "mode"
  - "parseHookOutput"
  - "stopReason"
  - "appendHookInvoked"
  - "appendHookResult"
  - "stderrSummary"
  - "matcherDiagnostic"
search_regex: "(?i)(json|shell|PreStepDecision|SurfaceEventType|PreToolDecision|PostToolDecision|decision|mode)"
---

# 0139. dsh-hook-protocol --- the shared Claude Code / Codex hook wire-protocol core — implementation context

## Open this when

The hooks subsystem ships two bridge plugins: one that runs a user's existing Claude Code (CC) hooks, one for Codex hooks. The reference implementations (~/repos/refs/claude-code, ~/repos/refs/codex) show a decisive fact: Codex deliberately reimplements a SUBSET of the CC hook protocol. Its engine reads the same hooks.json, uses the same matcher-group shape, the same exit-code/structured-stdout output contract, and the same command-hook execution model --- Codex's source even names the engine after Claude's and comments where it "intentionally diverges." So the two bridges would otherwise duplicate the bulk.

## Source decision

A new packages/hooks/ group with hook-protocol as a pure library. It owns four primitive families and the hook/ session events; each bridge plugin (dsh-hooks-claude-code, dsh-hooks-codex) owns what genuinely differs. Shared (here): Matcher --- matcherDiagnostic(pattern, mode) and matchesMatcher(pattern, query, mode). The ONE axis the dialects differ on is collapsed to the mode parameter: claude treats a pure [A-Za-z0-9_|]+ pattern as a literal (pipe = exact-match alternation) and anything else as a regex; codex is always an unanchored regex. Match-all on absent/''/''.

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/feature/2026-06-30-hook-protocol-lib.md](../02-notes/implemented/feature/2026-06-30-hook-protocol-lib.md)
- Pinned source: [.agents/notes/implemented/feature/2026-06-30-hook-protocol-lib.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/feature/2026-06-30-hook-protocol-lib.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`packages/shell/shell/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/shell/shell/src/index.ts) | package entry point | Core file in the package named by the note: `packages/shell/shell`. | `named-package-member` |
| [`packages/shell/shell/src/types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/shell/shell/src/types.ts) | public types and contract | Core file in the package named by the note: `packages/shell/shell`. | `named-package-member` |
| [`packages/shell/shell/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/shell/shell/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/shell/shell`. | `named-package-member` |
| [`packages/hooks/hooks-codex/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/hooks/hooks-codex/src/index.ts) | package entry point | Entry point or contract under the directory named by the note: `packages/hooks`. Core file in the package named by the note: `packages/hooks/hooks-codex`. | `named-directory-member, named-package-member, symbol-definition` |
| [`packages/hooks/hook-protocol/src/merge.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/hooks/hook-protocol/src/merge.ts) | runtime implementation | Entry point or contract under the directory named by the note: `packages/hooks`. Core file in the package named by the note: `packages/hooks/hook-protocol`. | `named-directory-member, named-package-member, symbol-definition` |
| [`packages/hooks/hook-protocol/src/types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/hooks/hook-protocol/src/types.ts) | public types and contract | Entry point or contract under the directory named by the note: `packages/hooks`. Core file in the package named by the note: `packages/hooks/hook-protocol`. | `named-directory-member, named-package-member, symbol-definition` |
| [`packages/hooks/hook-protocol/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/hooks/hook-protocol/src/index.ts) | package entry point | Core file in the package named by the note: `packages/hooks/hook-protocol`. | `named-package-member` |
| [`packages/hooks/hook-protocol/src/events.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/hooks/hook-protocol/src/events.ts) | runtime implementation | Entry point or contract under the directory named by the note: `packages/hooks`. Core file in the package named by the note: `packages/hooks/hook-protocol`. | `named-directory-member, named-package-member, symbol-definition` |
| [`packages/hooks/hook-protocol/src/matcher.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/hooks/hook-protocol/src/matcher.ts) | runtime implementation | Entry point or contract under the directory named by the note: `packages/hooks`. Core file in the package named by the note: `packages/hooks/hook-protocol`. | `named-directory-member, named-package-member, symbol-definition` |
| [`packages/hooks/hooks-codex/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/hooks/hooks-codex/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/hooks/hooks-codex`. | `named-package-member` |
| [`packages/hooks/hook-protocol/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/hooks/hook-protocol/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/hooks/hook-protocol`. | `named-package-member` |
| [`packages/hooks/hooks-claude-code/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/hooks/hooks-claude-code/src/index.ts) | package entry point | Core file in the package named by the note: `packages/hooks/hooks-claude-code`. | `named-package-member` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `json` | `const` | [`packages/client/modules/src/index.ts:169`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/modules/src/index.ts#L169) | `const json = JSON.stringify(graph).replaceAll('<', '\\u003c')` |
| `shell` | `const` | [`packages/client/web/src/boot.tsx:121`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/web/src/boot.tsx#L121) | `const shell = this.ctx.get('appShell')` |
| `PreStepDecision` | `type` | [`packages/core/agent/src/runtime-types.ts:53`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent/src/runtime-types.ts#L53) | `export type PreStepDecision =` |
| `SurfaceEventType` | `type` | [`packages/core/session/src/types.ts:343`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/types.ts#L343) | `export type SurfaceEventType =` |
| `PreToolDecision` | `type` | [`packages/core/tools/src/index.ts:588`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/index.ts#L588) | `export type PreToolDecision =` |
| `PostToolDecision` | `type` | [`packages/core/tools/src/index.ts:597`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/index.ts#L597) | `export type PostToolDecision =` |
| `decision` | `const` | [`packages/core/tools/src/index.ts:1743`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/index.ts#L1743) | `const decision = await this.ctx.waterfall(` |
| `mode` | `const` | [`packages/e2b/fs-e2b/src/index.ts:535`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/e2b/fs-e2b/src/index.ts#L535) | `const mode = existing === undefined ? 0o600 : existing.mode & 0o777` |
| `parseHookOutput` | `function` | [`packages/hooks/hook-protocol/src/codec.ts:59`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/hooks/hook-protocol/src/codec.ts#L59) | `export function parseHookOutput(exitCode: number \| undefined, stdout: string, stderr: string, expectedEventName?: string): HookOutput {` |
| `stopReason` | `const` | [`packages/hooks/hook-protocol/src/codec.ts:100`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/hooks/hook-protocol/src/codec.ts#L100) | `const stopReason = str(parsed, 'stopReason')` |
| `appendHookInvoked` | `function` | [`packages/hooks/hook-protocol/src/events.ts:75`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/hooks/hook-protocol/src/events.ts#L75) | `export function appendHookInvoked(session: Session, invocation: HookInvocation): void {` |
| `appendHookResult` | `function` | [`packages/hooks/hook-protocol/src/events.ts:92`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/hooks/hook-protocol/src/events.ts#L92) | `export function appendHookResult(session: Session, record: HookResultRecord): void {` |
| `stderrSummary` | `const` | [`packages/hooks/hook-protocol/src/events.ts:94`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/hooks/hook-protocol/src/events.ts#L94) | `const stderrSummary = summarizeStderr(output.stderr, record.stderrSummaryMaxChars)` |
| `matcherDiagnostic` | `function` | [`packages/hooks/hook-protocol/src/matcher.ts:37`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/hooks/hook-protocol/src/matcher.ts#L37) | `export function matcherDiagnostic(matcher: string \| undefined, mode: MatcherMode): string \| undefined {` |
| `matchesMatcher` | `function` | [`packages/hooks/hook-protocol/src/matcher.ts:57`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/hooks/hook-protocol/src/matcher.ts#L57) | `export function matchesMatcher(matcher: string \| undefined, query: string, mode: MatcherMode): boolean {` |
| `MergedHookOutcome` | `interface` | [`packages/hooks/hook-protocol/src/merge.ts:15`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/hooks/hook-protocol/src/merge.ts#L15) | `export interface MergedHookOutcome {` |

### Tests and executable evidence

- [`packages/core/tools/tests/tools.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/tests/tools.spec.ts) — A test under the owning area exercises or imports `PreToolDecision`. A test under the owning area exercises or imports `PostToolDecision`.
- [`packages/core/tools/tests/scoped.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/tests/scoped.spec.ts) — A test under the owning area exercises or imports `PreToolDecision`.
- [`packages/core/session/tests/surface.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/tests/surface.spec.ts) — A test under the owning area exercises or imports `SurfaceEventType`.
- [`packages/core/tools/tests/code-mode.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/tests/code-mode.spec.ts) — A test under the owning area exercises or imports `PostToolDecision`.
- [`packages/hooks/hooks-codex/tests/bridge.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/hooks/hooks-codex/tests/bridge.spec.ts) — A test under the owning area exercises or imports `dsh-hooks-codex`. A test under the owning area exercises or imports `codex`.
- [`packages/hooks/hooks-codex/tests/config.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/hooks/hooks-codex/tests/config.spec.ts) — A test under the owning area exercises or imports `dsh-hooks-codex`. A test under the owning area exercises or imports `codex`.
- [`packages/hooks/hook-protocol/tests/merge.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/hooks/hook-protocol/tests/merge.spec.ts) — A test under the owning area exercises or imports `HookOutput`. A test under the owning area exercises or imports `systemMessage`.
- [`packages/hooks/hook-protocol/tests/codec.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/hooks/hook-protocol/tests/codec.spec.ts) — A test under the owning area exercises or imports `parseHookOutput`. A test under the owning area exercises or imports `systemMessage`.

## How to read the implementation

1. Start with [`packages/shell/shell/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/shell/shell/src/index.ts) because it has the strongest evidence link to the note.
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

- Tags: `class/feature`, `concern/boundary`, `concern/cancellation-timeout`, `concern/lifecycle`, `concern/ownership`, `concern/schema-types`, `concern/trust`, `domain/build-release`, `domain/configuration`, `domain/context`, `domain/extensions`, `domain/llm`, `domain/observability`, `domain/protocols`, `domain/security`, `domain/session-state`, `domain/shell-terminal`, `domain/storage`, `lifecycle/implemented`, `mechanism/event-sourcing`, `mechanism/projection`, `mechanism/registry`
- Aliases: `json`, `shell`, `PreStepDecision`, `SurfaceEventType`, `PreToolDecision`, `PostToolDecision`, `decision`, `mode`, `parseHookOutput`, `stopReason`, `appendHookInvoked`, `appendHookResult`, `stderrSummary`, `matcherDiagnostic`
- Regex: `(?i)(json|shell|PreStepDecision|SurfaceEventType|PreToolDecision|PostToolDecision|decision|mode)`

```bash
rg -n --pcre2 "(?i)(json|shell|PreStepDecision|SurfaceEventType|PreToolDecision|PostToolDecision|decision|mode)" source-deepseek-harness
rg -l --fixed-strings "class/feature" 03-agent-context
```

## Connected notes

- **`source-link`** — [0453. Tighten the hook-protocol contract --- dialect, discarded fields, double defaults, and lib-owned `hook/result` semantics](0453-tighten-the-hook-protocol-contract-dialect-discarded-fields-double-defau.md): The source note links to this decision directly.
- **`source-link`** — [0517. Pre-tool input rewrite --- a consistent design](0517-pre-tool-input-rewrite-a-consistent-design.md): The source note links to this decision directly.
- **`shares-code-with`** — [0287. Plugin configuration in the web settings page](0287-plugin-configuration-in-the-web-settings-page.md): Shares source implementation: `packages/shell/shell/src/index.ts`, `packages/shell/shell/src/invariant.ts`.
- **`shares-code-with`** — [0066. The subprocess service is its own seam under the bash executors (`dsh-subprocess` / `dsh-subprocess-local`)](0066-the-subprocess-service-is-its-own-seam-under-the-bash-executors-dsh-subp.md): Shares source implementation: `packages/shell/shell/src/index.ts`, `packages/shell/shell/src/invariant.ts`.
- **`shares-code-with`** — [0095. One ordering for configuration sources, and what a discovered file may not decide](0095-one-ordering-for-configuration-sources-and-what-a-discovered-file-may-no.md): Shares source implementation: `packages/shell/shell/src/index.ts`, `packages/shell/shell/src/invariant.ts`.
- **`shares-code-with`** — [0193. tmux-location context](0193-tmux-location-context.md): Shares source implementation: `packages/shell/shell/src/index.ts`, `packages/shell/shell/src/invariant.ts`.
- **`shares-code-with`** — [0009. Session persistence as an abstract service over the existing `SessionEvent`](0009-session-persistence-as-an-abstract-service-over-the-existing-sessioneven.md): Shares source implementation: `packages/shell/shell/src/index.ts`, `packages/shell/shell/src/invariant.ts`.
- **`shares-code-with`** — [0146. The subprocess sandbox --- confinement seam, native runners, escalation, and per-session modes](0146-the-subprocess-sandbox-confinement-seam-native-runners-escalation-and-pe.md): Shares source implementation: `packages/shell/shell/src/index.ts`, `packages/shell/shell/src/types.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0139-dsh-hook-protocol-the-shared-claude-code-codex-hook-wire-protocol-core.md`.
